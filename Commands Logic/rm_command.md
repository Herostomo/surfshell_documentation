# rm Command Implementation

## Purpose

The `rm` command is used to delete a file or an empty directory from the filesystem. It is a simplified implementation of the Linux `rm` command using the C++17 `<filesystem>` library.

---

## Function Signature

```cpp
void rm(const std::vector<std::string>& args)
```

The function receives a vector of command-line arguments where:

- `args[0]` → Path of the file or empty directory to be removed.

---

## Implementation Logic

### 1. Validate Input

```cpp
if(args.empty())
{
    std::cout << "rm <path> missing";
    return;
}
```

- Checks whether the user has provided a path.
- If no path is supplied, an error message is displayed.
- The function exits immediately.

---

### 2. Remove the File or Directory

```cpp
std::filesystem::remove(args[0]);
```

The `std::filesystem::remove()` function attempts to delete the specified path.

It can:

- Delete a regular file.
- Delete an empty directory.
- Return `true` if deletion is successful.
- Return `false` if the file or directory does not exist.

In this implementation, the return value is ignored.

---

### 3. Exception Handling

```cpp
catch(const std::exception& e)
{
    std::cerr
        << "rm"
        << e.what()
        << "\n";
}
```

Any filesystem-related exception (such as permission denied or attempting to remove a non-empty directory) is caught, and the corresponding error message is displayed.

Example:

```
rm: Permission denied
```

or

```
rm: Directory not empty
```

depending on the operating system.

---

## Flow Diagram

```
User enters rm command
          │
          ▼
Receive arguments
          │
          ▼
Arguments empty?
      ┌───────┐
 Yes  │ Print │
      │ Error │
      └───┬───┘
          │
        Return
          │
 No       ▼
Call filesystem::remove()
          │
          ▼
Deletion attempted
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
| Remove file or empty directory | O(1)* |

\*Actual execution time depends on the operating system and underlying filesystem.

---

## Example Usage

### Remove a file

```
rm notes.txt
```

Output:

```
(File deleted successfully)
```

---

### Remove an empty directory

```
rm Documents
```

Output:

```
(Directory deleted successfully)
```

---

### Missing Argument

```
rm
```

Output:

```
rm <path> missing
```

---

### Attempt to Remove a Non-Empty Directory

```
rm Projects
```

Output:

```
rm: Directory not empty
```

---

## C++ Features Used

- `std::vector`
- `std::filesystem::remove()`
- Exception handling (`try-catch`)
- `std::cout`
- `std::cerr`

---

## Limitations

- Can only remove files or **empty directories**.
- Does not support recursive deletion (`rm -r`).
- Does not support force deletion (`rm -f`).
- Does not verify whether the specified path exists before attempting deletion.
- Ignores the boolean return value of `std::filesystem::remove()`, so no message is displayed if the path does not exist.

---

## Summary

The `rm` command implementation validates the user input, attempts to remove a specified file or empty directory using the C++17 `<filesystem>` library, and handles filesystem-related exceptions. It provides a simple cross-platform implementation suitable for a custom terminal application.