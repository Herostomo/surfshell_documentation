# Terminal Project Architecture Documentation

## Introduction

The terminal project is organized into multiple source and header files. Each file is responsible for a specific task such as user input handling, command parsing, command dispatching, or command implementation. This modular architecture improves readability, maintainability, and scalability.

---

# File Structure

```text
main.cpp
commands.h
commands.cpp
commands_parser.h
commands_parser.cpp
commands_dispatcher.h
commands_dispatcher.cpp
```

---

# 1. main.cpp

## Purpose

`main.cpp` serves as the entry point of the application and contains the main execution loop of the terminal.

## Responsibilities

- Display the terminal prompt.
    
- Read user input.
    
- Pass the input to the command parser.
    
- Send the parsed command to the dispatcher.
    
- Continue execution until the user exits the terminal.
    

## Execution Flow

```text
User Input
    ↓
Parser
    ↓
Dispatcher
    ↓
Command Execution
```

The main file does not contain command implementations. Its only responsibility is controlling the overall program flow.

---

# 2. commands.h

## Purpose

`commands.h` contains declarations of all terminal commands.

## Responsibilities

- Declare available command functions.
    
- Provide a common interface for other files.
    
- Hide implementation details from the rest of the program.
    

## Example

```cpp
void ls(const std::vector<std::string>& args);
void cp(const std::vector<std::string>& args);
```

By including this header, other files can use these functions without knowing how they are implemented.

---

# 3. commands.cpp

## Purpose

`commands.cpp` contains the actual implementation of terminal commands.

## Responsibilities

Implement all command logic such as:
Eg:

- ls
    
- cp
    
- mv
    
- mkdir
    
- rm
    
- pwd
    
## Example

```cpp
void ls(const std::vector<std::string>& args)
{
    // implementation
}
```

This file contains the real functionality behind every command.

---

# 4. commands_parser.h

## Purpose

`commands_parser.h` contains declarations related to parsing user input.

## Responsibilities

- Define structures used for parsed commands.
    
- Declare parser-related functions.
    

## Example

```cpp
struct ParsedCommand
{
    std::string command;
    std::vector<std::string> args;
};

ParsedCommand parseCommand(const std::string& input);
```

The parser converts raw text entered by the user into a structured format that the terminal can understand.

---

# 5. commands_parser.cpp

## Purpose

`commands_parser.cpp` contains the actual parsing logic.

## Responsibilities

- Extract the command name.
    
- Extract command arguments.
    
- Create a ParsedCommand object.
    

## Example

User enters:

```text
ls C:/Users/Kshitij/Documents
```

The parser converts it into:

```cpp
command = "ls"

args =
{
    "C:/Users/Kshitij/Documents"
}
```

The dispatcher can then use this structured information to determine which command should be executed.

---

# 6. commands_dispatcher.h

## Purpose

`commands_dispatcher.h` contains declarations related to command dispatching.

## Responsibilities

- Define dispatcher functions.
    
- Provide an interface between parsed commands and command execution.
    

## Example

```cpp
void dispatchCommand(const ParsedCommand& cmd);
```

The dispatcher acts as a decision-making layer between the parser and command implementations.

---

# 7. commands_dispatcher.cpp

## Purpose

`commands_dispatcher.cpp` contains the actual command dispatching logic.

## Responsibilities

- Match command names.
    
- Call the corresponding command implementation.
    
- Handle unknown commands.
    

## Example

```cpp
if(cmd.command == "ls")
{
    ls(cmd.args);
}
else if(cmd.command == "cp")
{
    cp(cmd.args);
}
else
{
    std::cout << "Unknown command";
}
```

This file acts as the bridge between user commands and their implementations.

---

# Complete Execution Flow

Suppose the user enters:

```text
ls C:/Users/Kshitij
```

The execution proceeds as follows:

### Step 1: Input Collection

`main.cpp` reads the user input.

```text
ls C:/Users/Kshitij
```

### Step 2: Command Parsing

`commands_parser.cpp` converts the raw text into:

```cpp
command = "ls"

args =
{
    "C:/Users/Kshitij"
}
```

### Step 3: Command Dispatching

`commands_dispatcher.cpp` checks the command name.

```cpp
if(command == "ls")
```

and calls:

```cpp
ls(args);
```

### Step 4: Command Execution

`commands.cpp` executes the `ls()` function.

### Step 5: Output

The contents of the specified directory are displayed to the user.

---

# Architecture Diagram

```text
                 User Input
                      │
                      ▼
                 main.cpp
                      │
                      ▼
          commands_parser.cpp
                      │
                      ▼
      ParsedCommand Structure
                      │
                      ▼
       commands_dispatcher.cpp
                      │
      ┌───────────────┴───────────────┐
      ▼                               ▼
   ls(args)                        cp(args)
      │                               │
      ▼                               ▼
 commands.cpp                  commands.cpp
      │
      ▼
    Output
```

---

# Benefits of This Design

## Separation of Concerns

Each file has a single responsibility.

|File|Responsibility|
|---|---|
|main.cpp|Program execution|
|commands.h|Command declarations|
|commands.cpp|Command implementations|
|commands_parser.h|Parser declarations|
|commands_parser.cpp|Parser implementation|
|commands_dispatcher.h|Dispatcher declarations|
|commands_dispatcher.cpp|Dispatcher implementation|

---

## Scalability

Adding a new command requires only three changes:

### Step 1

Declare the command in `commands.h`

```cpp
void mkdir(const std::vector<std::string>& args);
```

### Step 2

Implement the command in `commands.cpp`

```cpp
void mkdir(const std::vector<std::string>& args)
{
    // implementation
}
```

### Step 3

Register the command in `commands_dispatcher.cpp`

```cpp
else if(cmd.command == "mkdir")
{
    mkdir(cmd.args);
}
```

No modifications are required in the parser or main execution loop.

This makes the architecture easy to maintain and extend as additional terminal commands are added.

