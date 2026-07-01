# history Command Implementation

## Purpose

The `history` command displays a list of previously executed commands during the current terminal session. Each command is shown with a sequential index, allowing users to review their command history, similar to the Linux `history` command.

---

## Global Data Structure

```cpp
extern std::vector<std::string> command_history;
```

The command history is maintained in a global vector named `command_history`.

Each element of the vector stores one command exactly as entered by the user.

Using the `extern` keyword allows the `history` command to access the command history defined in another source file without creating a duplicate copy.

---

## Function Signature

```cpp
void history_command(const std::vector<std::string>&)
```

The function accepts a vector of command-line arguments but does not use them, since the `history` command does not require any additional parameters.

---

## Implementation Logic

### 1. Check Whether History Exists

```cpp
if(command_history.empty())
{
    std::cout << "No commands in history.\n";
    return;
}
```

The function first checks whether any commands have been stored.

If the history vector is empty, an informative message is displayed and execution terminates.

---

### 2. Traverse the History Vector

```cpp
for(size_t i = 0; i < command_history.size(); i++)
```

The function iterates through every command stored in the history vector.

Each iteration corresponds to one previously executed command.

---

### 3. Display Command Number and Text

```cpp
std::cout
    << i + 1
    << " "
    << command_history[i]
    << '\n';
```

For each command:

- A one-based index (`i + 1`) is displayed.
- The original command entered by the user is printed.

Example:

```
1 pwd
2 ls
3 mkdir Projects
4 cd Projects
```

---

## Flow Diagram

```
User enters history command
             │
             ▼
Access command history
             │
             ▼
History empty?
      ┌────────────┐
 Yes  │ Print      │
      │ "No        │
      │ history"   │
      └─────┬──────┘
            │
          Return
            │
 No         ▼
Iterate through
history vector
            │
            ▼
Print index and
stored command
            │
            ▼
Repeat until all
commands displayed
            │
            ▼
Function End
```

---

## Time Complexity

Let **n** be the number of stored commands.

| Operation | Complexity |
|-----------|------------|
| Check if empty | O(1) |
| Display history | O(n) |

Overall Time Complexity:

```
O(n)
```

Space Complexity:

```
O(1)
```

(The command uses the existing history vector and does not allocate additional storage.)

---

## Example Usage

### Display Command History

```
history
```

Output:

```
1 pwd
2 ls
3 mkdir Projects
4 cd Projects
5 touch notes.txt
6 cat notes.txt
```

---

### Empty History

```
history
```

Output:

```
No commands in history.
```

---

## C++ Features Used

- `extern`
- `std::vector`
- `std::string`
- `size_t`
- `for` loop
- `std::cout`

---

## Limitations

- Displays only commands executed during the current terminal session.
- Command history is stored in memory and is not saved to disk.
- Does not support clearing the history.
- Does not support Linux `history` features such as command replay (`!n`), filtering, timestamps, or persistent history across sessions.
- Ignores any command-line arguments passed to the function.

---

## Summary

The `history` command implementation accesses a global vector containing previously executed commands and displays them in chronological order with sequential numbering. It checks for an empty history before printing the stored commands, providing a simple and efficient command history feature for the custom terminal application.