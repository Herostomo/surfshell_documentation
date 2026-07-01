# echo Command Implementation

## Purpose

The `echo` command is used to display text on the terminal or write text to a file using output redirection. It supports standard output, overwrite redirection (`>`), and append redirection (`>>`), similar to the Linux `echo` command.

---

## Function Signature

```cpp
void echo(const std::vector<std::string>& args)
```

The function receives a vector of command-line arguments containing the text to print and, optionally, redirection operators.

---

## Implementation Logic

### 1. Check for Empty Input

```cpp
if (args.empty())
{
    std::cout << '\n';
    return;
}
```

- If no arguments are supplied, the command simply prints a blank line.
- The function then terminates.

---

### 2. Search for Output Redirection

```cpp
for (size_t i = 0; i < args.size(); ++i)
{
    if (args[i] == ">" || args[i] == ">>")
```

The function scans all arguments to determine whether output should be redirected.

Supported operators:

- `>` → Overwrite the destination file.
- `>>` → Append to the destination file.

If neither operator is found, the text is printed to the terminal.

---

### 3. Validate the Filename

```cpp
if (i + 1 >= args.size())
{
    std::cerr << "echo: missing filename\n";
    return;
}
```

After detecting a redirection operator, the next argument must be the destination filename.

If no filename is provided, an error message is displayed and execution stops.

---

### 4. Open the Destination File

```cpp
std::ofstream file(
    filename,
    args[i] == ">>" ? std::ios::app : std::ios::out
);
```

The file is opened in one of two modes:

- `std::ios::out` → Creates or overwrites the file (`>`).
- `std::ios::app` → Opens the file for appending (`>>`).

---

### 5. Verify File Access

```cpp
if (!file)
{
    std::cerr << "echo: cannot open file\n";
    return;
}
```

If the file cannot be opened (for example, due to insufficient permissions or an invalid path), an error message is displayed.

---

### 6. Write Text to the File

```cpp
for (size_t j = 0; j < i; ++j)
{
    file << args[j];

    if (j != i - 1)
        file << ' ';
}

file << '\n';
```

Only the arguments before the redirection operator are written to the file.

A single space is inserted between consecutive words, followed by a newline.

Example:

```
echo Hello World > output.txt
```

Contents of `output.txt`:

```
Hello World
```

---

### 7. Print to the Terminal

If no redirection operator is detected, the command prints all supplied arguments.

```cpp
for (size_t i = 0; i < args.size(); ++i)
{
    std::cout << args[i];

    if (i != args.size() - 1)
        std::cout << ' ';
}

std::cout << '\n';
```

Words are separated by spaces, and a newline is printed at the end.

---

## Flow Diagram

```
User enters echo command
            │
            ▼
Receive arguments
            │
            ▼
Arguments empty?
      ┌─────────────┐
 Yes  │ Print blank │
      │ line        │
      └──────┬──────┘
             │
           Return
             │
 No          ▼
Search for > or >>
             │
             ▼
Redirection found?
      ┌──────────────┐
 No   │ Print text   │
      │ to terminal  │
      └──────┬───────┘
             │
             ▼
        Function End

 Yes
  │
  ▼
Filename present?
      │
      ├── No → Print error
      │
      ▼
Open file
      │
      ▼
File opened?
      │
      ├── No → Print error
      │
      ▼
Write text to file
      │
      ▼
Function End
```

---

## Time Complexity

| Operation | Complexity |
|-----------|------------|
| Search for redirection | O(n) |
| Print text | O(n) |
| Write text to file | O(n) |

where **n** is the number of command-line arguments.

---

## Example Usage

### Print to Terminal

```
echo Hello World
```

Output:

```
Hello World
```

---

### Create or Overwrite a File

```
echo Hello World > output.txt
```

Contents of `output.txt`:

```
Hello World
```

---

### Append to a File

```
echo Welcome >> output.txt
```

Updated contents:

```
Hello World
Welcome
```

---

### Missing Filename

```
echo Hello >
```

Output:

```
echo: missing filename
```

---

### Invalid File

```
echo Hello > /protected/file.txt
```

Output:

```
echo: cannot open file
```

---

## C++ Features Used

- `std::vector`
- `std::ofstream`
- `std::ios::out`
- `std::ios::app`
- Conditional (ternary) operator
- `std::cout`
- `std::cerr`

---

## Limitations

- Supports only single-file output redirection.
- Does not support input redirection (`<`) or pipes (`|`).
- Does not interpret escape sequences such as `\n` or `\t`.
- Does not support Linux options like `-n` or `-e`.
- Treats all arguments literally without handling quotation marks or environment variable expansion.

---

## Summary

The `echo` command implementation prints text to the terminal or redirects it to a file using the `>` (overwrite) and `>>` (append) operators. It validates filenames, handles file opening errors, preserves spacing between arguments, and provides a simple implementation of shell-style output redirection for the custom terminal application.