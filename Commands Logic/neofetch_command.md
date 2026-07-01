# neofetch Command Implementation

## Purpose

The `neofetch` command displays basic system information along with a simple ASCII art logo. It provides a concise overview of the terminal environment, including the operating system name, application version, compiler, and current working directory. The implementation is inspired by the Linux **Neofetch** utility.

---

## Function Signature

```cpp
void neofetch(const std::vector<std::string>&)
```

The function accepts a vector of command-line arguments but does not use them, as the command requires no additional parameters.

---

## Implementation Logic

### 1. Display ASCII Art

```cpp
std::cout
<< "\033[36m"
<< R"(

       /\
      /  \
     /++++\
    /  ++  \
   /++++++++\
  /          \

)"
<< "\033[0m";
```

The command first prints a custom ASCII art logo.

A **raw string literal** (`R"( ... )"`) is used so that the artwork can be written without escaping special characters.

The ANSI escape sequence:

```cpp
"\033[36m"
```

changes the text color to **cyan**, while:

```cpp
"\033[0m"
```

restores the terminal's default formatting.

---

### 2. Display Operating System Information

```cpp
std::cout
<< "\033[1;32mOS:\033[0m Kshitij Terminal\n";
```

The operating system (or terminal environment) name is displayed.

The label **OS:** is printed in bold green using ANSI escape sequences.

---

### 3. Display Version

```cpp
<< "\033[1;32mVersion:\033[0m 1.0\n";
```

The current version of the terminal application is displayed.

---

### 4. Display Compiler Information

```cpp
<< "\033[1;32mCompiler:\033[0m GCC\n";
```

The compiler used to build the terminal application is shown.

---

### 5. Display Current Working Directory

```cpp
<< "\033[1;32mDirectory:\033[0m "
<< std::filesystem::current_path()
<< "\n";
```

The command retrieves the current working directory using:

```cpp
std::filesystem::current_path()
```

and displays its absolute path.

---

## ANSI Escape Sequences Used

| Sequence | Purpose |
|----------|---------|
| `\033[36m` | Set text color to cyan. |
| `\033[1;32m` | Set text to bold green. |
| `\033[0m` | Reset terminal formatting. |

---

## Flow Diagram

```
User enters neofetch command
               │
               ▼
Print ASCII art
               │
               ▼
Set cyan color
               │
               ▼
Reset formatting
               │
               ▼
Print OS information
               │
               ▼
Print application version
               │
               ▼
Print compiler information
               │
               ▼
Retrieve current directory
               │
               ▼
Display directory path
               │
               ▼
Function End
```

---

## Time Complexity

| Operation | Complexity |
|-----------|------------|
| Print system information | O(1) |

The command performs a fixed number of operations regardless of the filesystem size.

---

## Example Usage

### Display System Information

```
neofetch
```

Output:

```
       /\
      /  \
     /++++\
    /  ++  \
   /++++++++\
  /          \

OS:        Kshitij Terminal
Version:   1.0
Compiler:  GCC
Directory: C:\Projects\SurfShell
```

(The output is colorized when executed in a terminal that supports ANSI escape sequences.)

---

## C++ Features Used

- Raw string literals (`R"( ... )"`)
- ANSI escape sequences
- `std::filesystem::current_path()`
- `std::cout`

---

## Limitations

- Displays only a small set of predefined information.
- Operating system name, version, and compiler are hardcoded rather than detected dynamically.
- Does not display hardware information such as CPU, RAM, GPU, disk usage, uptime, or kernel version like the original Linux **Neofetch** utility.
- Requires ANSI escape sequence support for colored output.

---

## Summary

The `neofetch` command implementation displays a custom ASCII art logo followed by basic terminal information, including the application name, version, compiler, and current working directory. It uses ANSI escape sequences to produce colored output and `std::filesystem::current_path()` to retrieve the current directory, providing a lightweight system information utility inspired by the popular Linux **Neofetch** command.