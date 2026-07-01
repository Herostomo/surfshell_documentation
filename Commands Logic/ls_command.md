### LS Command Implementation Logic

The `ls` command is used to display all files and folders present in a directory.

Before writing the code, the expected beh
avior of the command is:

1. List all files and folders present in a specified directory.
    
2. If no directory is specified, list the contents of the current working directory.
    
3. Display each file or folder name on a separate line.
    

To implement this functionality, the C++17 `<filesystem>` library provides the `std::filesystem::directory_iterator` class. This iterator takes a directory path as input and allows traversal through all files and folders contained within that directory.

The implementation begins by setting the default path to the current directory:

```cpp
std::filesystem::path path = ".";
```

The `"."` symbol represents the current working directory.

Next, the command checks whether the user has provided any arguments:

```cpp
if(!args.empty())
{
    path = args[0];
}
```

If an argument exists, the first argument is treated as the directory path. For example:

```text
ls C:/Users/name/Documents
```

Here, `args[0]` contains:

```text
C:/Users/name/Documents
```

and the command will display the contents of that directory instead of the current directory.

Finally, the directory contents are traversed using:

```cpp
for(const auto& entry : std::filesystem::directory_iterator(path))
{
    std::cout << entry.path().filename().string() << std::endl;
}
```

`directory_iterator` visits each file and folder inside the specified directory. For every entry, `filename()` extracts only the file or folder name, and it is printed on a new line.

Exception handling is also included to catch errors such as:

- Directory does not exist.
    
- Invalid path provided.
    
- Permission denied.
    

This ensures the terminal does not crash when an invalid directory is supplied.