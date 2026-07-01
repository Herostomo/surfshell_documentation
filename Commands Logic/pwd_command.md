
# PWD Command Implementation Logic

## Overview

The `pwd` (Print Working Directory) command is used to display the absolute path of the current working directory.

It allows the user to know their current location in the file system.

---

## Expected Behaviour

Example:

```text
pwd
```

Output:

```text
C:/Users/Kshitij/terms_good
```

---

## Implementation Logic

The filesystem library provides:

```cpp
std::filesystem::current_path()
```

without any arguments.

This version returns the current working directory as a path object.

Example:

```cpp
std::cout << std::filesystem::current_path() << '\n';
```

Output:

```text
C:/Users/Kshitij/terms_good
```

---

## Command Flow

```text
User Input
    │
    ▼
pwd
    │
    ▼
current_path()
    │
    ▼
Current Directory Path Returned
    │
    ▼
Display Path to User
```

---

## Summary
    
### pwd

- Used to display the current directory.
    
- Uses `current_path()`.
    
- Returns the absolute path of the current working directory.
    
- Does not require any arguments.