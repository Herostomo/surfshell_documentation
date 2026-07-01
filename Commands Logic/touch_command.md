# touch Command Implementation

## Purpose

The `touch` command is used to create a new empty file if it does not already exist. If the file already exists, it is opened in append mode and then immediately closed, leaving its contents unchanged.

---

## Function Signature

```cpp
void touch(const std::vector<std::string>& args)
```

The function receives a vector of command-line arguments where:

- `args[0]` → Name or path of the file to be created.

---

## Implementation Logic

### 1. Validate Input

```cpp
if(args.empty())
{
    std::cout << "touch <path> missing";
    return;
}
```

- Checks whether the user has supplied a file path.
- If no argument is provided, an error message is displayed.
- The function exits immediately.

---

### 2. Create or Open the File

```cpp
std::ofstream file(args[0], std::ios::app);
```

An output file stream is created using append mode (`std::ios::app`).

Behavior:

- If the file does not exist, a new empty file is created.
- If the file already exists, it is opened for appending.
- Existing file contents remain unchanged.

---

### 3. Close the File

```cpp
file.close();
```

After opening (or creating) the file, it is immediately closed.

This ensures:

- File resources are released.
- The newly created file is saved properly.

---

### 4. Exception Handling

```cpp
catch(const std::exception& e)
{
    std::cerr
        << "touch"
        << e.what()
        << "\n";
}
```

Any exception encountered during file creation or opening is caught, and the corresponding error message is displayed.

Example:

```
touch: Permission denied
```

or

```
touch: Invalid path
```

depending on the operating system.

---

## Flow Diagram

```
User enters touch command
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
Open file in append mode
           │
           ▼
File exists?
     ┌───────────────┐
 Yes │ Open file     │
     │ without       │
     │ modifying it  │
     └──────┬────────┘
            │
 No         ▼
Create new file
            │
            ▼
Close file
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
| Create/Open file | O(1)* |

\*Actual execution time depends on the operating system and underlying filesystem.

---

## Example Usage

### Create a New File

```
touch notes.txt
```

Output:

```
(File created successfully)
```

---

### Open an Existing File

```
touch notes.txt
```

Output:

```
(File remains unchanged)
```

---

### Missing Argument

```
touch
```

Output:

```
touch <path> missing
```

---

## C++ Features Used

- `std::vector`
- `std::ofstream`
- `std::ios::app`
- Exception handling (`try-catch`)
- `std::cout`
- `std::cerr`

---

## Limitations

- Does not update the file's last modified timestamp like the Linux `touch` command.
- Creates only regular files.
- Does not verify whether the file was successfully opened using `file.is_open()`.
- Does not support Linux options such as `-a`, `-m`, `-c`, or specifying custom timestamps.

---

## Summary

The `touch` command implementation validates the user input, creates a new file (or opens an existing one) using `std::ofstream` in append mode, immediately closes the file, and handles exceptions gracefully. It provides a simple and portable implementation for creating empty files in a custom terminal application.