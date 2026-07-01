# mkdir Command Implementation

## Purpose
The `mkdir` command is used to create a new directory (folder) at the specified path. It behaves similarly to the Linux `mkdir` command for creating a single directory.

---

## Function Signature

```cpp
void mkdir(const std::vector<std::string>& args)
```

The function receives a vector of command-line arguments where:

- `args[0]` → Directory path to be created.

---

## Implementation Logic

### 1. Validate Input

```cpp
if(args.empty())
{
    std::cout << "mkdir <path> missing";
    return;
}
```

- Checks whether the user supplied a directory name.
- If no argument is provided, an error message is displayed.
- Function exits immediately.

---

### 2. Create the Directory

```cpp
std::filesystem::create_directory(args[0]);
```

The C++17 `<filesystem>` library provides `create_directory()`.

It:

- Creates a single directory.
- Returns `true` if the directory was successfully created.
- Returns `false` if the directory already exists.

---

### 3. Handle Existing Directory

```cpp
if(!std::filesystem::create_directory(args[0]))
{
    std::cerr << "mkdir: directory already exists\n";
}
```

If `create_directory()` returns `false`, the implementation assumes the directory already exists and informs the user.

---

### 4. Exception Handling

```cpp
catch(const std::exception& e)
{
    std::cerr
        << "mkdir"
        << e.what()
        << "\n";
}
```

Any filesystem-related exception (such as invalid path or insufficient permissions) is caught and its message is printed.

Example:

```
mkdir: Permission denied
```

or

```
mkdir: Invalid argument
```

depending on the operating system.

---

## Flow Diagram

```
User enters mkdir command
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
Call create_directory()
          │
          ▼
Directory created?
      ┌──────────┐
 Yes  │ Success  │
      └────┬─────┘
           │
           ▼
        Function End

 No
  │
  ▼
Print "directory already exists"
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
| Create directory | O(1)* |

\*Actual execution time depends on the operating system and underlying filesystem.

---

## Example Usage

### Create a directory

```
mkdir Projects
```

Output:

```
Directory created successfully
```

---

### Directory already exists

```
mkdir Projects
```

Output:

```
mkdir: directory already exists
```

---

### Missing Argument

```
mkdir
```

Output:

```
mkdir <path> missing
```

---

## C++ Features Used

- `std::vector`
- `std::filesystem::create_directory()`
- Exception handling (`try-catch`)
- `std::cerr`
- `std::cout`

---

## Limitations

- Creates only a **single directory**.
- Does not support creating nested directories (e.g., `mkdir a/b/c` if parent folders do not exist).
- Does not support Linux options like `-p`, `-v`, or setting permissions.
- Assumes `create_directory()` returning `false` means the directory already exists, although other conditions may also cause failure without throwing an exception.

---

## Summary

The `mkdir` command implementation validates user input, attempts to create a single directory using the C++17 `<filesystem>` library, reports if the directory already exists, and safely handles filesystem exceptions. It provides a simple cross-platform implementation suitable for a custom terminal application.