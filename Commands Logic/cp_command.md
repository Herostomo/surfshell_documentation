# cp Command Implementation

## Purpose

The `cp` command is used to copy a file from one location to another. It utilizes the C++17 `<filesystem>` library and supports overwriting an existing destination file, providing functionality similar to the Linux `cp` command.

---

## Function Signature

```cpp
void cp(const std::vector<std::string>& args)
```

The function receives a vector of command-line arguments where:

- `args[0]` → Source file.
- `args[1]` → Destination file.

---

## Implementation Logic

### 1. Validate Input

```cpp
if(args.size() < 2)
{
    std::cerr << "cp: missing source or destination\n";
    return;
}
```

The function first verifies that both the source and destination paths have been provided.

If either argument is missing, an error message is displayed and execution terminates.

---

### 2. Copy the File

```cpp
std::filesystem::copy_file(
    args[0],
    args[1],
    std::filesystem::copy_options::overwrite_existing
);
```

The `std::filesystem::copy_file()` function copies the contents of the source file to the destination.

The copy operation uses:

```cpp
std::filesystem::copy_options::overwrite_existing
```

which allows the destination file to be replaced if it already exists.

---

### 3. Display Success Message

```cpp
std::cout << "File copied successfully.\n";
```

If the copy operation completes without throwing an exception, a confirmation message is displayed.

---

### 4. Handle Filesystem Exceptions

```cpp
catch(const std::filesystem::filesystem_error& e)
{
    std::cerr << "cp: " << e.what() << '\n';
}
```

Filesystem-related exceptions are caught separately to provide detailed information about copy failures.

Common reasons include:

- Source file does not exist.
- Destination path is invalid.
- Permission denied.
- Attempting to copy a directory using `copy_file()`.

---

### 5. Handle General Exceptions

```cpp
catch(const std::exception& e)
{
    std::cerr << "cp: " << e.what() << '\n';
}
```

Any remaining standard C++ exceptions are caught to prevent unexpected program termination.

---

## Flow Diagram

```
User enters cp command
            │
            ▼
Receive arguments
            │
            ▼
Two arguments provided?
      ┌────────────┐
 No   │ Print      │
      │ Error      │
      └─────┬──────┘
            │
          Return
            │
 Yes        ▼
Call filesystem::copy_file()
            │
            ▼
Copy successful?
      ┌────────────┐
 No   │ Exception  │
      │ Thrown     │
      └─────┬──────┘
            │
            ▼
Print error message
            │
            ▼
Function End

 Yes
  │
  ▼
Print success message
  │
  ▼
Function End
```

---

## Time Complexity

| Operation | Complexity |
|-----------|------------|
| Copy file | O(n) |

where **n** is the size of the source file being copied.

---

## Example Usage

### Copy a File

```
cp report.txt backup.txt
```

Output:

```
File copied successfully.
```

---

### Overwrite an Existing File

```
cp report.txt backup.txt
```

If `backup.txt` already exists, it is replaced with the contents of `report.txt`.

Output:

```
File copied successfully.
```

---

### Missing Arguments

```
cp report.txt
```

Output:

```
cp: missing source or destination
```

---

### Source File Does Not Exist

```
cp test.txt backup.txt
```

Output (example):

```
cp: filesystem error: cannot copy file...
```

(The exact message depends on the operating system.)

---

## C++ Features Used

- `std::vector`
- `std::filesystem::copy_file()`
- `std::filesystem::copy_options`
- `std::filesystem::filesystem_error`
- Exception handling (`try-catch`)
- `std::cout`
- `std::cerr`

---

## Limitations

- Supports copying **files only**; directories cannot be copied using `copy_file()`.
- Always overwrites the destination file if it already exists.
- Does not preserve file metadata such as permissions, timestamps, or ownership.
- Does not support recursive directory copying.
- Does not support Linux options such as `-r`, `-i`, `-p`, `-v`, or `-u`.

---

## Summary

The `cp` command implementation validates the source and destination paths, copies the specified file using `std::filesystem::copy_file()`, and automatically overwrites any existing destination file through the `overwrite_existing` option. It provides clear success and error messages while handling filesystem-related exceptions, offering a simple and portable file-copying utility for the custom terminal application.