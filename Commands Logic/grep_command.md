# grep Command Implementation

## Purpose

The `grep` command is used to search for a specified text pattern within a file. It scans the file line by line and prints every line containing the pattern along with its corresponding line number. This implementation provides basic substring matching similar to the Linux `grep` command.

---

## Function Signature

```cpp
void grep(const std::vector<std::string>& args)
```

The function receives a vector of command-line arguments where:

- `args[0]` → Pattern to search for.
- `args[1]` → File in which the pattern should be searched.

---

## Implementation Logic

### 1. Validate Input

```cpp
if(args.size() < 2)
{
    std::cerr << "grep: usage: grep <pattern> <file>\n";
    return;
}
```

The function verifies that both a search pattern and a filename have been supplied.

If either argument is missing, a usage message is displayed and execution terminates.

---

### 2. Store Input Parameters

```cpp
std::string pattern = args[0];
std::string filename = args[1];
```

The search pattern and filename are extracted from the command-line arguments for later use.

---

### 3. Open the File

```cpp
std::ifstream file(filename);
```

An input file stream is created to open the specified file in read mode.

---

### 4. Verify File Access

```cpp
if(!file)
{
    std::cerr << "grep: cannot open file '" << filename << "'\n";
    return;
}
```

If the file cannot be opened (for example, because it does not exist or due to insufficient permissions), an error message is displayed and the function exits.

---

### 5. Read the File Line by Line

```cpp
std::string line;
size_t line_number = 1;
```

The file is processed one line at a time using `std::getline()`.

A line counter is initialized to keep track of the current line number.

---

### 6. Search for the Pattern

```cpp
if(line.find(pattern) != std::string::npos)
```

For each line:

- `std::string::find()` searches for the specified pattern.
- If the pattern exists within the line, the function returns its position.
- If the pattern is not found, `std::string::npos` is returned.

---

### 7. Display Matching Lines

```cpp
std::cout
    << line_number
    << ": "
    << line
    << '\n';
```

Whenever a match is found:

- The line number is printed.
- The complete line containing the pattern is displayed.

Example:

```
15: int main()
```

---

### 8. Update the Line Counter

```cpp
++line_number;
```

The line number is incremented after processing each line.

---

## Flow Diagram

```
User enters grep command
             │
             ▼
Receive arguments
             │
             ▼
Pattern and filename provided?
      ┌───────────────┐
 No   │ Print usage   │
      │ message       │
      └──────┬────────┘
             │
           Return
             │
 Yes         ▼
Open file
             │
             ▼
File opened?
      ┌────────────┐
 No   │ Print      │
      │ Error      │
      └─────┬──────┘
            │
          Return
            │
 Yes        ▼
Read file line by line
            │
            ▼
Search for pattern
            │
            ▼
Pattern found?
      │
      ├── No → Read next line
      │
      └── Yes
             │
             ▼
Print line number
and matching line
             │
             ▼
Continue until EOF
             │
             ▼
Function End
```

---

## Time Complexity

Let:

- **n** = Number of lines in the file.
- **m** = Average length of each line.

| Operation | Complexity |
|-----------|------------|
| Read file | O(n) |
| Pattern search | O(m) per line |

Overall Time Complexity:

```
O(n × m)
```

---

## Example Usage

### Search for a Word

```
grep main program.cpp
```

Output:

```
5: int main()
42: return main_result;
```

---

### Search for a Variable

```
grep count data.txt
```

Output:

```
12: int count = 10;
25: total_count++;
```

---

### Missing Arguments

```
grep hello
```

Output:

```
grep: usage: grep <pattern> <file>
```

---

### File Not Found

```
grep hello test.txt
```

Output:

```
grep: cannot open file 'test.txt'
```

---

## C++ Features Used

- `std::vector`
- `std::string`
- `std::string::find()`
- `std::ifstream`
- `std::getline()`
- `std::cout`
- `std::cerr`

---

## Limitations

- Performs simple substring matching only.
- Does not support regular expressions.
- Search is case-sensitive.
- Searches only one file at a time.
- Does not support Linux `grep` options such as `-i` (ignore case), `-n`, `-r`, `-v`, `-c`, or wildcard file searches.

---

## Summary

The `grep` command implementation searches a text file line by line using `std::getline()` and performs substring matching with `std::string::find()`. Whenever the specified pattern is found, the matching line and its corresponding line number are displayed. The implementation includes input validation and file access checks, providing a straightforward and efficient text-search utility for the custom terminal application.