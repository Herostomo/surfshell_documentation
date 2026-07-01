# help Command Implementation

## Purpose

The `help` command provides built-in documentation for all commands supported by the terminal. It displays either a list of all available commands or detailed information about a specific command, making it easier for users to understand the functionality and usage of the terminal.

---

## Data Structure Used

### `CommandInfo` Structure

```cpp
struct CommandInfo
{
    std::string name;
    std::string description;
    std::string usage;
    std::string details;
};
```

Each command is represented using a `CommandInfo` structure containing:

| Member | Description |
|---------|-------------|
| `name` | Command name (e.g., `ls`) |
| `description` | Short description of the command |
| `usage` | Correct syntax for using the command |
| `details` | Detailed explanation of the command's functionality |

---

### Command Database

```cpp
std::vector<CommandInfo> commands;
```

A `std::vector` stores the information for every supported command.

Each entry contains:

- Command name
- Brief description
- Usage syntax
- Detailed explanation

This acts as an internal help database for the terminal.

---

## Function Signature

```cpp
void help(const std::vector<std::string>& args)
```

The function receives a vector of command-line arguments where:

- No arguments → Display all available commands.
- One argument → Display detailed information for the specified command.

---

## Implementation Logic

### 1. Check Whether an Argument Is Supplied

```cpp
if(args.empty())
```

The function first determines whether the user requested:

- General help
- Help for a specific command

---

### 2. Display All Commands

```cpp
std::cout << "Available Commands:\n\n";
```

If no argument is provided, the function iterates through every entry in the command database.

```cpp
for(const auto& cmd : commands)
{
    std::cout
        << cmd.name
        << " - "
        << cmd.description
        << '\n';
}
```

Only the command name and short description are displayed.

Finally, the user is informed that more detailed help is available.

```cpp
std::cout
    << "\nType 'help <command>' for detailed information.\n";
```

---

### 3. Retrieve the Requested Command

```cpp
std::string commandName = args[0];
```

If an argument exists, it is treated as the name of the command for which help is requested.

---

### 4. Search the Command Database

```cpp
for(const auto& cmd : commands)
{
    if(cmd.name == commandName)
```

The vector is searched sequentially until a matching command is found.

---

### 5. Display Detailed Information

When a match is found, the following information is displayed:

```cpp
std::cout << "\nCommand : " << cmd.name << '\n';
std::cout << "Description : " << cmd.description << '\n';
std::cout << "Usage : " << cmd.usage << '\n';
std::cout << "Details : " << cmd.details << '\n';
```

The output includes:

- Command name
- Description
- Usage syntax
- Detailed explanation

---

### 6. Handle Unknown Commands

If no matching command exists:

```cpp
std::cout
    << "help: command '"
    << commandName
    << "' not found\n";
```

An informative error message is displayed.

---

## Flow Diagram

```
User enters help command
             │
             ▼
Receive arguments
             │
             ▼
Arguments supplied?
      ┌───────────────┐
 No   │ Display list  │
      │ of commands   │
      └──────┬────────┘
             │
             ▼
Display usage hint
             │
             ▼
Function End

 Yes
  │
  ▼
Extract command name
  │
  ▼
Search command database
  │
  ▼
Command found?
      │
      ├── Yes
      │      │
      │      ▼
      │ Display detailed
      │ information
      │
      └── No
             │
             ▼
Print "command not found"
             │
             ▼
Function End
```

---

## Time Complexity

Let **n** be the number of supported commands.

| Operation | Complexity |
|-----------|------------|
| Display all commands | O(n) |
| Search for one command | O(n) |

Since the implementation performs a linear search through the command database, the worst-case time complexity is **O(n)**.

---

## Example Usage

### Display All Commands

```
help
```

Output:

```
Available Commands:

ls - List files and directories
cd - Change current directory
pwd - Print current working directory
...
help - Show help information

Type 'help <command>' for detailed information.
```

---

### Display Help for a Specific Command

```
help ls
```

Output:

```
Command : ls
Description : List files and directories
Usage : ls [path]
Details : Displays all files and folders in the specified directory. If no path is given, current directory is used.
```

---

### Unknown Command

```
help abc
```

Output:

```
help: command 'abc' not found
```

---

## C++ Features Used

- `struct`
- `std::vector`
- Range-based `for` loop
- `std::string`
- `std::cout`
- `const` references

---

## Limitations

- Command lookup is performed using a linear search, which becomes less efficient as the number of commands grows.
- Command names are case-sensitive.
- Supports help for one command at a time.
- Help information is statically stored in the source code and must be updated manually when new commands are added.
- Does not support command aliases or fuzzy matching for misspelled commands.

---

## Summary

The `help` command implementation provides an integrated documentation system for the custom terminal by maintaining a structured database of command metadata. It can display either a summary of all available commands or detailed information for a specific command, making the terminal easier to learn and use while demonstrating the use of C++ structures, vectors, sequential search, and formatted console output.