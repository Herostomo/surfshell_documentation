# stat Command Implementation

## Purpose

The `stat` command is used to display basic information about a file or directory. It checks whether the specified path exists, determines its type, and displays relevant metadata such as file size for regular files. The implementation uses the C++17 `<filesystem>` library.

---

## Function Signature

```cpp
void stat(const std::vector<std::string>& args)
```

The function receives a vector of command-line arguments where:

- `args[0]` → Path of the file or directory whose information is to be displayed.

---

## Implementation Logic

### 1. Validate Input

```cpp
if(args.empty())
{
    std::cerr << "stat: missing file operand\n";
    return;
}
```

The function first checks whether a path has been provided.

If no argument is supplied, an error message is displayed and execution terminates.

---

### 2. Create a Filesystem Path

```cpp
std::filesystem::path p(args[0]);
```

The supplied string is converted into a `std::filesystem::path` object, which simplifies filesystem operations.

---

### 3. Check Whether the Path Exists

```cpp
if(!std::filesystem::exists(p))
{
    std::cerr << "stat: file does not exist\n";
    return;
}
```

The command verifies that the specified file or directory exists.

If it does not exist, an error message is displayed.

---

### 4. Retrieve Filesystem Status

```cpp
auto status = std::filesystem::status(p);
```

The `status()` function obtains metadata about the path, including its type (regular file, directory, symbolic link, etc.).

---

### 5. Display the Path

```cpp
std::cout << "Path: " << p << '\n';
```

The full path supplied by the user is displayed before additional information.

---

### 6. Identify the File Type

The command checks the type of the filesystem object.

#### Regular File

```cpp
if(std::filesystem::is_regular_file(status))
{
    std::cout << "Type: Regular File\n";
```

If the object is a regular file:

- The type is displayed.
- The file size is obtained using `std::filesystem::file_size()`.

```cpp
std::cout
    << "Size: "
    << std::filesystem::file_size(p)
    << " bytes\n";
```

---

#### Directory

```cpp
else if(std::filesystem::is_directory(status))
{
    std::cout << "Type: Directory\n";
}
```

If the path refers to a directory, only its type is displayed.

---

#### Symbolic Link

```cpp
else if(std::filesystem::is_symlink(status))
{
    std::cout << "Type: Symbolic Link\n";
}
```

If the path is a symbolic link, the corresponding type is displayed.

---

#### Other Filesystem Objects

```cpp
else
{
    std::cout << "Type: Other\n";
}
```

Any other filesystem object (such as sockets, FIFOs, or special device files) is classified as **Other**.

---

## Flow Diagram

```
User enters stat command
            │
            ▼
Receive path
            │
            ▼
Path provided?
      ┌────────────┐
 No   │ Print      │
      │ Error      │
      └─────┬──────┘
            │
          Return
            │
 Yes        ▼
Create filesystem path
            │
            ▼
Path exists?
      ┌────────────┐
 No   │ Print      │
      │ Error      │
      └─────┬──────┘
            │
          Return
            │
 Yes        ▼
Retrieve file status
            │
            ▼
Determine object type
            │
            ▼
Regular File?
      │
      ├── Yes → Print file size
      │
      ├── Directory → Print type
      │
      ├── Symbolic Link → Print type
      │
      └── Other → Print type
            │
            ▼
Function End
```

---

## Time Complexity

| Operation | Complexity |
|-----------|------------|
| Check file existence | O(1)* |
| Retrieve file status | O(1)* |
| Obtain file size | O(1)* |

\*Actual execution time depends on the operating system and underlying filesystem.

---

## Example Usage

### Display Information About a File

```
stat report.txt
```

Output:

```
Path: report.txt
Type: Regular File
Size: 2456 bytes
```

---

### Display Information About a Directory

```
stat Documents
```

Output:

```
Path: Documents
Type: Directory
```

---

### Missing Argument

```
stat
```

Output:

```
stat: missing file operand
```

---

### File Does Not Exist

```
stat test.txt
```

Output:

```
stat: file does not exist
```

---

## C++ Features Used

- `std::vector`
- `std::filesystem::path`
- `std::filesystem::exists()`
- `std::filesystem::status()`
- `std::filesystem::is_regular_file()`
- `std::filesystem::is_directory()`
- `std::filesystem::is_symlink()`
- `std::filesystem::file_size()`
- `std::cout`
- `std::cerr`

---

## Limitations

- Displays only basic filesystem information.
- Reports file size only for regular files.
- Does not display permissions, ownership, creation time, modification time, inode number, or timestamps like the Linux `stat` command.
- Does not resolve symbolic links to display target information.
- Does not include exception handling for filesystem-related errors.

---

## Summary

The `stat` command implementation validates the supplied path, checks whether it exists, retrieves its filesystem status using the C++17 `<filesystem>` library, identifies the object type, and displays basic metadata such as file size for regular files. It provides a lightweight alternative to the Linux `stat` utility for the custom terminal application.