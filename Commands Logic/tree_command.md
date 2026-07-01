# tree Command Implementation

## Purpose

The `tree` command displays the hierarchical structure of directories and files in a tree-like format. Starting from a specified directory (or the current working directory by default), it recursively traverses the filesystem and prints every file and subdirectory using indentation to represent the directory hierarchy.

---

## Function Signatures

```cpp
void printTree(const std::filesystem::path& path,
               const std::string& prefix = "");
```

```cpp
void tree(const std::vector<std::string>& args);
```

The implementation consists of two functions:

- `tree()` → Validates the input and selects the root directory.
- `printTree()` → Recursively traverses and displays the directory structure.

---

# printTree() Function

## Purpose

The `printTree()` function performs the recursive traversal of the directory hierarchy and prints every file and folder.

### Parameters

| Parameter | Description |
|-----------|-------------|
| `path` | Current directory being traversed. |
| `prefix` | Indentation used to visualize directory depth. |

---

## Implementation Logic

### 1. Iterate Through Directory Entries

```cpp
for(const auto& entry :
    std::filesystem::directory_iterator(path))
```

The function iterates through every file and subdirectory contained within the current directory.

---

### 2. Display the Entry

```cpp
std::cout
    << prefix
    << "├── "
    << entry.path().filename().string()
    << '\n';
```

Each entry is printed using:

- The current indentation (`prefix`)
- A tree connector (`├──`)
- The filename

Example:

```
├── Documents
├── notes.txt
├── Images
```

---

### 3. Check for Subdirectories

```cpp
if(std::filesystem::is_directory(entry)
   && !std::filesystem::is_symlink(entry))
```

The function determines whether the current entry is:

- A directory
- Not a symbolic link

Skipping symbolic links prevents infinite recursion caused by cyclic directory references.

---

### 4. Recursive Traversal

```cpp
printTree(
    entry.path(),
    prefix + "│   "
);
```

If the current entry is a directory, `printTree()` calls itself recursively.

The indentation prefix is extended for each new directory level, producing the tree-like appearance.

---

# tree() Function

## Purpose

The `tree()` function selects the root directory, validates the input, and initiates the recursive traversal.

---

### 1. Select the Root Directory

```cpp
std::filesystem::path root = ".";
```

By default, the current working directory is used.

If the user specifies a directory:

```cpp
if(!args.empty())
{
    root = args[0];
}
```

the supplied path becomes the new root.

---

### 2. Verify Path Existence

```cpp
if(!std::filesystem::exists(root))
{
    std::cerr << "tree: path does not exist\n";
    return;
}
```

The command verifies that the requested directory exists before traversal begins.

---

### 3. Display the Root Directory

```cpp
std::cout << root.string() << '\n';
```

The selected root directory is printed first.

---

### 4. Start Recursive Traversal

```cpp
printTree(root);
```

The recursive helper function is invoked to display the complete directory hierarchy.

---

## Flow Diagram

```
User enters tree command
            │
            ▼
Receive arguments
            │
            ▼
Directory specified?
      ┌────────────┐
 No   │ Use current│
      │ directory  │
      └─────┬──────┘
            │
 Yes        ▼
Use specified path
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
Print root directory
            │
            ▼
Call printTree()
            │
            ▼
Traverse directory
            │
            ▼
Print each entry
            │
            ▼
Entry is directory?
      │
      ├── No → Continue
      │
      └── Yes
             │
             ▼
Recursive call
             │
             ▼
Continue until all
directories processed
             │
             ▼
Function End
```

---

## Time Complexity

Let **n** be the total number of filesystem entries (files and directories).

| Operation | Complexity |
|-----------|------------|
| Recursive traversal | O(n) |

Overall Time Complexity:

```
O(n)
```

Space Complexity:

```
O(d)
```

where **d** is the maximum depth of the directory hierarchy due to recursive function calls.

---

## Example Usage

### Display Current Directory

```
tree
```

Output:

```
.
├── Documents
│   ├── report.txt
│   ├── notes.txt
├── Images
│   ├── logo.png
├── main.cpp
```

---

### Display Another Directory

```
tree Projects
```

Output:

```
Projects
├── SurfShell
│   ├── commands.cpp
│   ├── main.cpp
│   ├── README.md
```

---

### Invalid Directory

```
tree UnknownFolder
```

Output:

```
tree: path does not exist
```

---

## C++ Features Used

- `std::filesystem::path`
- `std::filesystem::directory_iterator`
- `std::filesystem::exists()`
- `std::filesystem::is_directory()`
- `std::filesystem::is_symlink()`
- Recursion
- Default function arguments
- `std::string`
- `std::cout`
- `std::cerr`

---

## Limitations

- Uses `std::filesystem::directory_iterator`, so directory entries are displayed in the order returned by the operating system rather than being alphabetically sorted.
- Does not display file metadata such as size or permissions.
- Does not count the total number of files and directories.
- Uses a simplified tree format and does not distinguish the last child with `└──`.
- Does not support Linux `tree` options such as `-L` (depth limit), `-a` (show hidden files), `-d` (directories only), or colorized output.

---

## Summary

The `tree` command implementation displays a hierarchical view of the filesystem by recursively traversing directories using `std::filesystem::directory_iterator`. The `tree()` function validates the input and selects the root directory, while the recursive `printTree()` function prints each file and subdirectory with indentation that reflects the directory structure. By skipping symbolic links during recursion, the implementation avoids cyclic traversal and provides a clear, tree-like visualization of the filesystem for the custom terminal application.