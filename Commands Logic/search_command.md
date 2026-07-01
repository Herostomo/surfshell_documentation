# search Command Implementation

## Purpose

The `search` command enables web searching directly from the terminal. It sends the user's query to the **Tavily Search API**, retrieves the top search results in JSON format, displays them in the terminal, and allows the user to open any selected result in the system's default web browser.

The implementation integrates several technologies:

- **libcurl** for HTTP communication
- **nlohmann/json** for JSON serialization and parsing
- **Windows Shell API** (`ShellExecuteA`) for opening web pages
- **C++17 `<filesystem>`** for locating the SSL certificate bundle

---

# Components

The implementation consists of three major parts:

1. `WriteCallback()` – Receives HTTP response data.
2. `search_tavily()` – Sends the search request and parses the response.
3. `search()` – Handles user interaction and opens the selected webpage.

---

# 1. WriteCallback()

## Purpose

```cpp
size_t WriteCallback(
    void* contents,
    size_t size,
    size_t nmemb,
    void* userp)
```

libcurl delivers downloaded data in small chunks.

`WriteCallback()` appends each chunk to a string that stores the complete HTTP response.

---

### Implementation

```cpp
((std::string*)userp)->append(
    (char*)contents,
    size * nmemb);
```

The downloaded bytes are appended to the response string.

The callback returns the number of processed bytes:

```cpp
return size * nmemb;
```

which informs libcurl that the data was handled successfully.

---

# 2. search_tavily()

## Purpose

```cpp
std::vector<SearchResult>
search_tavily(const std::string& query)
```

This function performs the complete communication with the Tavily Search API.

It:

- Creates an HTTP client.
- Builds a JSON request.
- Sends the request.
- Receives the JSON response.
- Extracts search results.
- Returns them as a vector.

---

## Step 1 – Initialize libcurl

```cpp
CURL* curl = curl_easy_init();
```

A CURL session is created.

If initialization fails:

```cpp
if(!curl)
```

the function immediately returns an empty result list.

---

## Step 2 – Configure HTTP Headers

```cpp
headers = curl_slist_append(
    headers,
    "Content-Type: application/json");
```

The request specifies that the payload is JSON.

A custom User-Agent header is also included:

```cpp
User-Agent: TerminalProject/1.0
```

---

## Step 3 – Build the JSON Request

```cpp
json body =
{
    {"api_key", "..."},
    {"query", query},
    {"max_results", 5},
    {"search_depth", "basic"}
};
```

The request contains:

| Field | Purpose |
|--------|----------|
| `api_key` | Tavily authentication key |
| `query` | User search query |
| `max_results` | Maximum number of returned results |
| `search_depth` | Search mode |

The JSON object is converted into a string using:

```cpp
body.dump();
```

---

## Step 4 – Configure the HTTP Request

The request URL is configured:

```cpp
CURLOPT_URL
```

```
https://api.tavily.com/search
```

Additional options include:

- SSL certificate location (`CURLOPT_CAINFO`)
- HTTP headers
- POST body
- POST body size
- Response callback
- Response destination

---

## Step 5 – Locate the SSL Certificate

```cpp
GetModuleFileNameA(...)
```

retrieves the executable's location.

The program then constructs:

```
ca-bundle.crt
```

using

```cpp
std::filesystem::path
```

so libcurl can verify the HTTPS connection.

---

## Step 6 – Send the Request

```cpp
curl_easy_perform(curl);
```

The HTTP POST request is transmitted to the Tavily API.

If communication fails:

```cpp
CURLE_OK
```

is checked and the corresponding CURL error message is displayed.

---

## Step 7 – Parse the JSON Response

```cpp
auto parsed = json::parse(response);
```

The returned JSON string is parsed into a JSON object.

---

## Step 8 – Extract Search Results

If the response contains:

```cpp
results
```

each entry is converted into a `SearchResult`.

```cpp
r.title
r.url
```

These objects are stored inside a vector.

---

## Step 9 – Cleanup

After processing:

```cpp
curl_slist_free_all(headers);

curl_easy_cleanup(curl);
```

All allocated CURL resources are released.

The populated vector is then returned.

---

# 3. search()

## Purpose

```cpp
void search(
    const std::vector<std::string>& args)
```

This function manages user interaction.

It collects the search query, displays search results, accepts user input, and opens the selected webpage.

---

## Step 1 – Validate Input

```cpp
if(args.empty())
```

If no query is supplied:

```
Usage: search <query>
```

is displayed.

---

## Step 2 – Construct the Search Query

```cpp
for(const auto& arg : args)
{
    query += arg + " ";
}
```

Multiple command-line arguments are combined into one search string.

Example:

```
search machine learning tutorial
```

becomes

```
machine learning tutorial
```

---

## Step 3 – Perform Search

```cpp
auto results =
    search_tavily(query);
```

The request is sent to Tavily.

---

## Step 4 – Display Search Results

Each result is printed with a numbered index.

```cpp
[1] Title
[2] Title
...
```

ANSI escape sequences are used to color the result numbers.

---

## Step 5 – Receive User Selection

```cpp
std::cin >> choice;
```

The user selects one result.

Input validation checks:

- Invalid numeric input
- Numbers outside the valid range

---

## Step 6 – Open the Webpage

```cpp
ShellExecuteA(
    nullptr,
    "open",
    results[choice-1].url.c_str(),
    nullptr,
    nullptr,
    SW_SHOWNORMAL);
```

The selected URL is opened in the system's default web browser.

---

## Flow Diagram

```
User enters search command
             │
             ▼
Validate query
             │
             ▼
Build search string
             │
             ▼
Initialize CURL
             │
             ▼
Construct JSON request
             │
             ▼
Send POST request
to Tavily API
             │
             ▼
Receive JSON response
             │
             ▼
Parse search results
             │
             ▼
Display numbered list
             │
             ▼
User selects result
             │
             ▼
Validate selection
             │
             ▼
Open URL using
ShellExecuteA()
             │
             ▼
Function End
```

---

## Time Complexity

Let **n** be the number of search results returned.

| Operation | Complexity |
|-----------|------------|
| Build query | O(k) |
| Parse JSON | O(n) |
| Display results | O(n) |

The network request dominates the execution time and depends on internet latency rather than algorithmic complexity.

---

## Example Usage

### Search the Web

```
search c++ filesystem tutorial
```

Output:

```
Searching...

Results:

[1] C++ Filesystem Library
[2] cppreference filesystem
[3] Learn C++ Filesystem
[4] ...
```

User selects:

```
2
```

The selected webpage opens automatically in the default web browser.

---

### Missing Query

```
search
```

Output:

```
Usage: search <query>
```

---

### Invalid Selection

```
Select result: 10
```

Output:

```
Invalid choice
```

---

## Technologies Used

- **libcurl**
- **nlohmann/json**
- **Windows Shell API (`ShellExecuteA`)**
- **Windows API (`GetModuleFileNameA`)**
- **C++17 `<filesystem>`**
- **ANSI escape sequences**
- **HTTP POST**
- **JSON serialization and parsing**

---

## Limitations

- Requires an active internet connection.
- Requires a valid Tavily API key.
- Currently retrieves a maximum of five search results.
- Designed for Windows because it uses `ShellExecuteA()` and `GetModuleFileNameA()`.
- Opens webpages only in the system's default browser.
- Does not cache search results or support advanced search filters.

---

## Summary

The `search` command integrates a web search capability directly into the terminal by communicating with the **Tavily Search API**. It constructs a JSON request using **nlohmann/json**, sends it securely over HTTPS with **libcurl**, parses the returned JSON response, presents the results in a numbered list, and launches the selected webpage using the Windows Shell API. This implementation demonstrates the integration of networking, JSON processing, filesystem utilities, and operating system APIs to provide a seamless web-search experience within the custom terminal application.