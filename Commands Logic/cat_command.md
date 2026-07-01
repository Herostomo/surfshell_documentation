# cat Command Implementation

## Purpose

The `cat` command is used to display the contents of a text file on the terminal. It reads the file line by line and prints each line to the standard output, similar to the Linux `cat` command.

---

## Function Signature

```cpp
void cat(const std::vector<std::string>& args)
```

The function receives a vector of command-line arguments where:

- `args[0]` → Path of the file to be displayed.

---

## Implementation Logic

### 1. Validate Input

```cpp
if(args.empty())
{
    std::cout << "cat <path> missing";
    return;
}
```

- Checks whether the user has supplied a file path.
- If no argument is provided, an error message is displayed.
- The function exits immediately.

---

### 2. Open the File

```cpp
std::ifstream file(args[0]);
```

An input file stream (`std::ifstream`) is created to open the specified file in read mode.

---

### 3. Verify File Access

```cpp
if(!file)
{
    std::cerr << "cat: cannot open file\n";
    return;
}
```

If the file cannot be opened (for example, because it does not exist or due to insufficient permissions), an error message is displayed and the function terminates.

---

### 4. Read the File Line by Line

```cpp
std::string line;

while(std::getline(file, line))
{
    std::cout << line << std::endl;
}
```

The function continuously reads one line at a time using `std::getline()`.

For every successfully read line:

- The line is printed to the terminal.
- `std::endl` inserts a newline and flushes the output stream.

The loop continues until the end of the file is reached.

---

### 5. Exception Handling

```cpp
catch(std::exception& e)
{
    std::cerr
        << "cat"
        << e.what()
        << "\n";
}
```

Any exception generated during file handling is caught, and the corresponding error message is displayed.

Example:

```
cat: Permission denied
```

or

```
cat: Invalid argument
```

depending on the operating system.

---

## Flow Diagram

```
User enters cat command
           │
           ▼
Receive arguments
           │
           ▼
Arguments empty?
      ┌──────────┐
 Yes  │ Print    │
      │ Error    │
      └────┬─────┘
           │
         Return
           │
 No        ▼
Open file
           │
           ▼
File opened?
      ┌─────────────┐
 No   │ Print Error │
      └──────┬──────┘
             │
           Return
             │
 Yes         ▼
Read file line by line
             │
             ▼
Print each line
             │
             ▼
End of file?
      │
      ├── No → Read next line
      │
      ▼
Function End

Exceptions
    │
    ▼
Catch exception
    │
    ▼
Print error message
```

---

## Time Complexity

| Operation | Complexity |
|-----------|------------|
| Open file | O(1) |
| Read entire file | O(n) |

where **n** is the number of lines (or equivalently, proportional to the file size).

---

## Example Usage

### Display a File

```
cat notes.txt
```

Contents of `notes.txt`:

```
Welcome
This is SurfShell.
```

Output:

```
Welcome
This is SurfShell.
```

---

### Missing Argument

```
cat
```

Output:

```
cat <path> missing
```

---

### File Does Not Exist

```
cat test.txt
```

Output:

```
cat: cannot open file
```

---

## C++ Features Used

- `std::vector`
- `std::ifstream`
- `std::getline()`
- `std::string`
- `while` loop
- Exception handling (`try-catch`)
- `std::cout`
- `std::cerr`

---

## Limitations

- Displays only one file at a time.
- Supports text files only; binary files may produce unreadable output.
- Does not support concatenating multiple files.
- Does not support Linux options such as `-n`, `-b`, `-E`, `-T`, or `-A`.
- Uses `std::endl`, which flushes the output stream after every line and may be less efficient for very large files.

---

## Summary

The `cat` command implementation validates the user input, opens the specified file using `std::ifstream`, reads it line by line with `std::getline()`, and displays its contents on the terminal. It includes error handling for missing arguments, file access failures, and runtime exceptions, providing a simple cross-platform file viewing utility for the custom terminal application.