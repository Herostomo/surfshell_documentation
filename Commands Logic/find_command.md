# find Command Implementation

## Purpose

The `find` command is used to recursively search for a file or directory by name, starting from the current working directory. It traverses the directory tree and displays the absolute path of every matching file or folder. The implementation uses the C++17 `<filesystem>` library.

---

## Function Signature

```cpp
void find(const std::vector<std::string>& args)
```

The function receives a vector of command-line arguments where:

- `args[0]` → Name of the file or directory to search for.

---

## Implementation Logic

### 1. Validate Input

```cpp
if(args.empty())
{
    std::cerr << "Command argument not specified" << std::endl;
    return;
}
```

The function first checks whether a search target has been provided.

If no argument is supplied, an error message is displayed and execution terminates.

---

### 2. Store the Search Target

```cpp
std::string target = args[0];
```

The filename or directory name entered by the user is stored in a string for comparison during the search.

---

### 3. Traverse the Directory Tree

```cpp
for(const auto& entry :
    std::filesystem::recursive_directory_iterator(
        ".",
        std::filesystem::directory_options::skip_permission_denied))
```

The command recursively explores every file and subdirectory beginning from the current working directory (`"."`).

The option:

```cpp
std::filesystem::directory_options::skip_permission_denied
```

ensures that directories with insufficient permissions are skipped automatically instead of causing the program to terminate.

---

### 4. Compare File Names

```cpp
if(entry.path().filename() == target)
```

For every filesystem entry:

- The filename portion of the path is extracted.
- It is compared with the target name provided by the user.

Only exact matches are considered.

---

### 5. Display the Absolute Path

```cpp
std::cout
    << std::filesystem::absolute(entry.path())
    << '\n';
```

When a match is found, the complete absolute path is displayed.

Using absolute paths allows users to identify the exact location of every matching file or directory.

---

## Flow Diagram

```
User enters find command
             │
             ▼
Receive arguments
             │
             ▼
Search target provided?
      ┌────────────┐
 No   │ Print      │
      │ Error      │
      └─────┬──────┘
            │
          Return
            │
 Yes        ▼
Store target name
            │
            ▼
Recursively traverse
current directory
            │
            ▼
Compare filename
with target
            │
            ▼
Match found?
      │
      ├── No → Continue searching
      │
      └── Yes
             │
             ▼
Print absolute path
             │
             ▼
Continue until all
entries processed
             │
             ▼
Function End
```

---

## Time Complexity

Let **n** be the total number of files and directories under the current working directory.

| Operation | Complexity |
|-----------|------------|
| Recursive traversal | O(n) |
| Filename comparison | O(1) per entry |

Overall Time Complexity:

```
O(n)
```

Space Complexity:

```
O(1)
```

excluding the internal storage used by the filesystem iterator.

---

## Example Usage

### Search for a File

```
find report.txt
```

Output:

```
C:\Projects\SurfShell\Documents\report.txt
```

---

### Search for a Directory

```
find Images
```

Output:

```
C:\Projects\SurfShell\Assets\Images
```

---

### Missing Argument

```
find
```

Output:

```
Command argument not specified
```

---

### Multiple Matches

```
find config.json
```

Output:

```
C:\Projects\App1\config.json
C:\Projects\App2\config.json
C:\Projects\Test\config.json
```

---

## C++ Features Used

- `std::vector`
- `std::string`
- `std::filesystem::recursive_directory_iterator`
- `std::filesystem::directory_options`
- `std::filesystem::absolute()`
- `std::filesystem::path`
- Range-based `for` loop
- `std::cout`
- `std::cerr`

---

## Limitations

- Always begins searching from the current working directory (`"."`).
- Performs exact filename matching only; wildcard or regular expression searches are not supported.
- Search is case-sensitive, depending on the underlying filesystem.
- Does not support filtering by file extension, size, or modification date.
- Does not provide options similar to the Linux `find` command (e.g., `-name`, `-type`, `-size`, `-mtime`, or `-exec`).

---

## Summary

The `find` command implementation recursively traverses the directory tree starting from the current working directory using `std::filesystem::recursive_directory_iterator`. It compares each filename against the user-specified target and prints the absolute path of every matching file or directory. By using the `skip_permission_denied` option, the search continues even when inaccessible directories are encountered, providing a simple and portable file-search utility for the custom terminal application.