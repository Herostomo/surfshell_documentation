# clear Command Implementation

## Purpose

The `clear` command is used to clear the terminal screen and reposition the cursor to the top-left corner. It achieves this by printing ANSI escape sequences that are interpreted by terminals supporting ANSI control codes.

---

## Function Signature

```cpp
void clear()
```

The function takes no arguments and clears the terminal display.

---

## Implementation Logic

### 1. Print ANSI Escape Sequences

```cpp
std::cout << "\033[3J\033[2J\033[H";
```

The command outputs three ANSI escape sequences:

| Escape Sequence | Purpose |
|-----------------|---------|
| `\033[3J` | Clears the terminal's scrollback buffer (supported by many modern terminals). |
| `\033[2J` | Clears the entire visible screen. |
| `\033[H` | Moves the cursor to the home position (row 1, column 1). |

Together, these sequences provide behavior similar to the Linux `clear` command.

---

### 2. Flush the Output Buffer

```cpp
std::cout.flush();
```

The output buffer is flushed immediately to ensure that the ANSI escape sequences are sent to the terminal without delay.

Without flushing, the terminal might not update the display until additional output is produced.

---

## Flow Diagram

```
User enters clear command
            │
            ▼
Print ANSI escape sequences
            │
            ▼
Clear scrollback buffer
            │
            ▼
Clear terminal screen
            │
            ▼
Move cursor to top-left
            │
            ▼
Flush output buffer
            │
            ▼
Terminal screen cleared
```

---

## Time Complexity

| Operation | Complexity |
|-----------|------------|
| Print ANSI escape sequences | O(1) |

The command performs a constant amount of work regardless of the terminal size.

---

## Example Usage

### Clear the Terminal

```
clear
```

Result:

- All visible terminal output is cleared.
- The cursor moves to the top-left corner.
- In terminals supporting `\033[3J`, the scrollback history is also cleared.

---

## ANSI Escape Sequences Used

| Sequence | Description |
|----------|-------------|
| `\033` | Escape character (ASCII 27). |
| `[3J` | Clear scrollback buffer. |
| `[2J` | Clear entire screen. |
| `[H` | Move cursor to the home position. |

---

## C++ Features Used

- `std::cout`
- ANSI escape sequences
- `std::cout.flush()`

---

## Limitations

- Requires a terminal that supports ANSI escape sequences.
- Behavior of `\033[3J` (clearing the scrollback buffer) may vary depending on the terminal emulator.
- Does not work correctly in environments that do not interpret ANSI escape codes (such as older Windows Command Prompt versions without virtual terminal processing enabled).
- Clears only the terminal display; it does not affect program variables or application state.

---

## Summary

The `clear` command implementation clears the terminal screen by printing ANSI escape sequences that erase the visible display, optionally clear the scrollback buffer, and reposition the cursor to the top-left corner. The output stream is then flushed to ensure the command takes effect immediately, providing a lightweight and efficient screen-clearing utility for the custom terminal application.