# CD Command Implementation Logic

## Overview

The `cd` (Change Directory) command is used to navigate between directories in the file system. It changes the current working directory of the terminal process so that future commands operate relative to the new location.

---

## Expected Behaviour

### 1. Navigate Between Directories

The primary purpose of the `cd` command is to move from one directory to another.

Example:

```text
cd C:/Users/Kshitij/Documents
```

After execution, the current working directory becomes:

```text
C:/Users/Kshitij/Documents
```

---

### 2. Initial Working Directory

When the terminal starts, it automatically begins execution from a current working directory.

For example:

```text
C:/Users/Kshitij/terms_good
```

All relative path operations are performed with respect to this directory until the user changes it using the `cd` command.

---

### 3. Accepting a Path

To navigate to another location, the command requires a valid directory path.

Example:

```text
cd C:/Users/Kshitij/Documents
```

The provided path is passed as an argument to the command implementation.

```cpp
args[0]
```

contains:

```text
C:/Users/Kshitij/Documents
```

---

### 4. Missing Path Handling

If the user enters:

```text
cd
```

without specifying a path, the command should display an error message.

Example:

```text
cd: missing path
```

This prevents the program from attempting to access an invalid argument.

---

### 5. Moving to the Parent Directory

The special path:

```text
..
```

represents the parent directory.

Example:

Current directory:

```text
C:/Users/Kshitij/terms_good
```

Command:

```text
cd ..
```

Result:

```text
C:/Users/Kshitij
```

The filesystem library automatically understands the meaning of `".."`, so no special logic is required in the implementation.

The user can repeatedly use:

```text
cd ..
```

to move up one directory level at a time.

---

### 6. Using current_path()

The C++17 filesystem library provides the function:

```cpp
std::filesystem::current_path(const std::filesystem::path& p);
```

This function changes the current working directory to the path specified by `p`.

Example:

```cpp
std::filesystem::current_path(args[0]);
```

If:

```cpp
args[0] = "C:/Users/Kshitij/Documents";
```

then the current working directory becomes:

```text
C:/Users/Kshitij/Documents
```

Similarly, if:

```cpp
args[0] = "..";
```

then the current working directory becomes the parent directory.

---

## Command Flow

```text
User Input
    │
    ▼
cd C:/Users/Kshitij/Documents
    │
    ▼
Parser extracts:
command = "cd"
args[0] = "C:/Users/Kshitij/Documents"
    │
    ▼
current_path(args[0])
    │
    ▼
Current Working Directory Updated
```

---
Summary :
### cd

- Used to change the current directory.
    
- Accepts a directory path as input.
    
- Uses `current_path(path)`.
    
- Supports special paths such as `".."`.
    
- Displays an error when no path is provided.