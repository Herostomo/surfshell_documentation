# wc Command Implementation

## Purpose

The `wc` command is used to count the number of lines, words, and characters in a text file. It reads the file line by line, calculates these statistics, and displays the results. The implementation provides functionality similar to the Linux `wc` command.

---

## Function Signature

```cpp
void wc(const std::vector<std::string>& args)
```

The function receives a vector of command-line arguments where:

- `args[0]` → Path of the file whose statistics are to be calculated.

---

## Implementation Logic

### 1. Validate Input

```cpp
if(args.empty())
{
    std::cerr << "wc: missing filename\n";
    return;
}
```

The function first checks whether a filename has been supplied.

If no argument is provided, an error message is displayed and execution terminates.

---

### 2. Open the File

```cpp
std::ifstream file(args[0]);
```

An input file stream is created to open the specified file in read mode.

---

### 3. Verify File Access

```cpp
if(!file)
{
    std::cerr << "wc: cannot open file\n";
    return;
}
```

If the file cannot be opened, an error message is displayed and the function exits.

---

### 4. Initialize Counters

```cpp
size_t lines = 0;
size_t words = 0;
size_t characters = 0;
```

Three counters are maintained throughout the file traversal:

- **lines** → Total number of lines.
- **words** → Total number of words.
- **characters** → Total number of characters.

---

### 5. Read the File Line by Line

```cpp
std::string line;

while(std::getline(file, line))
```

The file is processed one line at a time using `std::getline()`.

---

### 6. Count Lines

```cpp
lines++;
```

Each successful call to `std::getline()` corresponds to one line in the file.

---

### 7. Count Characters

```cpp
characters += line.length() + 1;
```

The length of each line is added to the character count.

An additional character is counted for the newline (`'\n'`) that separates lines.

---

### 8. Count Words

```cpp
std::stringstream ss(line);
std::string word;

while(ss >> word)
{
    words++;
}
```

A `std::stringstream` is created for each line.

The extraction operator (`>>`) automatically separates words using whitespace as the delimiter.

Each extracted word increments the word counter.

---

### 9. Display the Results

```cpp
std::cout << "Lines: " << lines << '\n';
std::cout << "Words: " << words << '\n';
std::cout << "Characters: " << characters << '\n';
```

After processing the complete file, the total number of lines, words, and characters is displayed.

---

## Flow Diagram

```
User enters wc command
            │
            ▼
Receive filename
            │
            ▼
Filename provided?
      ┌────────────┐
 No   │ Print      │
      │ Error      │
      └─────┬──────┘
            │
          Return
            │
 Yes        ▼
Open file
            │
            ▼
File opened?
      ┌────────────┐
 No   │ Print      │
      │ Error      │
      └─────┬──────┘
            │
          Return
            │
 Yes        ▼
Initialize counters
            │
            ▼
Read file line by line
            │
            ▼
Increment line count
            │
            ▼
Add character count
            │
            ▼
Extract and count words
            │
            ▼
End of file?
      │
      ├── No → Continue reading
      │
      ▼
Display statistics
            │
            ▼
Function End
```

---

## Time Complexity

Let:

- **n** = Number of lines in the file.
- **m** = Average number of characters per line.

| Operation | Complexity |
|-----------|------------|
| Read file | O(n) |
| Count words | O(m) per line |

Overall Time Complexity:

```
O(total characters in file)
```

which is linear with respect to the size of the input file.

Space Complexity:

```
O(m)
```

where **m** is the maximum length of a single line.

---

## Example Usage

### Count File Statistics

```
wc notes.txt
```

Output:

```
Lines: 15
Words: 126
Characters: 834
```

---

### Missing Filename

```
wc
```

Output:

```
wc: missing filename
```

---

### File Does Not Exist

```
wc report.txt
```

Output:

```
wc: cannot open file
```

---

## C++ Features Used

- `std::vector`
- `std::ifstream`
- `std::getline()`
- `std::stringstream`
- `std::string`
- `std::cout`
- `std::cerr`

---

## Limitations

- Counts characters by adding `line.length() + 1` for each line, assuming a newline character follows every line. This may slightly differ from the actual file size if the last line does not end with a newline or if different newline conventions (e.g., `\r\n`) are used.
- Uses whitespace to separate words and does not account for punctuation or locale-specific rules.
- Processes one file at a time.
- Does not support Linux `wc` options such as `-l`, `-w`, `-c`, `-m`, or multiple file inputs.

---

## Summary

The `wc` command implementation reads a text file line by line using `std::getline()`, counts the total number of lines, words, and characters, and displays the computed statistics. Word counting is performed using `std::stringstream`, while character counting is based on the length of each line plus newline characters. The implementation provides a simple and efficient file statistics utility for the custom terminal application.