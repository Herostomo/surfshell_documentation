# diff Command Implementation

## Purpose

The `diff` command is used to compare two text files and display the differences between them. It identifies lines that are common to both files, lines removed from the first file, and lines added in the second file. The implementation is based on the **Longest Common Subsequence (LCS)** algorithm using dynamic programming.

---

## Function Signature

```cpp
void diff(const std::vector<std::string>& args)
```

The function receives a vector of command-line arguments where:

- `args[0]` → First file to compare.
- `args[1]` → Second file to compare.

---

## Implementation Logic

### 1. Validate Input

```cpp
if(args.size() < 2)
{
    std::cerr << "diff: missing file operands\n";
    return;
}
```

The function verifies that two filenames are supplied.

If either filename is missing, an error message is displayed and execution terminates.

---

### 2. Open Both Files

```cpp
std::ifstream file1(args[0]);
std::ifstream file2(args[1]);
```

Two input streams are created to open both files in read mode.

If either file cannot be opened, an appropriate error message is displayed.

```cpp
if(!file1)
{
    std::cerr << "diff: cannot open " << args[0] << '\n';
    return;
}

if(!file2)
{
    std::cerr << "diff: cannot open " << args[1] << '\n';
    return;
}
```

---

### 3. Read Both Files

```cpp
std::vector<std::string> A;
std::vector<std::string> B;
```

Each file is read line by line using `std::getline()`.

```cpp
while(std::getline(file1, line))
    A.push_back(line);

while(std::getline(file2, line))
    B.push_back(line);
```

After this step:

- `A` contains every line of the first file.
- `B` contains every line of the second file.

---

### 4. Build the Dynamic Programming Table

```cpp
std::vector<std::vector<int>> dp(
    n + 1,
    std::vector<int>(m + 1, 0)
);
```

A two-dimensional DP table is constructed where:

- `n` = Number of lines in the first file.
- `m` = Number of lines in the second file.

Each cell `dp[i][j]` stores the length of the **Longest Common Subsequence (LCS)** between:

- The first `i` lines of file A.
- The first `j` lines of file B.

The table is filled using:

```cpp
if(A[i - 1] == B[j - 1])
{
    dp[i][j] = dp[i - 1][j - 1] + 1;
}
else
{
    dp[i][j] = std::max(
        dp[i - 1][j],
        dp[i][j - 1]
    );
}
```

---

### 5. Backtrack Through the DP Table

After constructing the DP table, the algorithm traverses it backwards to reconstruct the differences.

```cpp
int i = n;
int j = m;
```

Three possible cases are encountered:

#### Common Line

```cpp
result.push_back("  " + A[i - 1]);
```

A line existing in both files is marked with two leading spaces.

Example:

```
  Hello World
```

---

#### Line Removed

```cpp
result.push_back("- " + A[i - 1]);
```

A line present only in the first file is prefixed with `-`.

Example:

```
- Old Line
```

---

#### Line Added

```cpp
result.push_back("+ " + B[j - 1]);
```

A line present only in the second file is prefixed with `+`.

Example:

```
+ New Line
```

---

### 6. Handle Remaining Lines

If one file contains additional lines after the other has been completely processed, they are added as insertions or deletions.

```cpp
while(i > 0)
{
    result.push_back("- " + A[i - 1]);
    i--;
}

while(j > 0)
{
    result.push_back("+ " + B[j - 1]);
    j--;
}
```

---

### 7. Reverse the Result

Since backtracking starts from the end of both files, the generated differences are in reverse order.

```cpp
std::reverse(result.begin(), result.end());
```

The vector is reversed before displaying the final output.

---

### 8. Display Differences

```cpp
for(const auto& entry : result)
{
    std::cout << entry << '\n';
}
```

Each comparison result is printed line by line.

---

### 9. Exception Handling

```cpp
catch(const std::exception& e)
{
    std::cerr << "diff: " << e.what() << '\n';
}
```

Any runtime exception is caught and displayed.

---

## Flow Diagram

```
User enters diff command
             │
             ▼
Receive filenames
             │
             ▼
Two files provided?
      ┌────────────┐
 No   │ Print      │
      │ Error      │
      └─────┬──────┘
            │
          Return
            │
 Yes        ▼
Open both files
            │
            ▼
Files opened?
      ┌────────────┐
 No   │ Print      │
      │ Error      │
      └─────┬──────┘
            │
          Return
            │
 Yes        ▼
Read both files
            │
            ▼
Construct LCS DP table
            │
            ▼
Backtrack through DP
            │
            ▼
Identify:
  Common lines
  Added lines
  Removed lines
            │
            ▼
Reverse result
            │
            ▼
Display differences
            │
            ▼
Function End
```

---

## Time Complexity

Let:

- **n** = Number of lines in the first file.
- **m** = Number of lines in the second file.

| Operation | Complexity |
|-----------|------------|
| Read both files | O(n + m) |
| Construct DP table | O(n × m) |
| Backtracking | O(n + m) |

Overall Time Complexity:

```
O(n × m)
```

Space Complexity:

```
O(n × m)
```

due to the dynamic programming table.

---

## Example Usage

### Compare Two Files

```
diff file1.txt file2.txt
```

Suppose:

**file1.txt**

```
Apple
Banana
Orange
```

**file2.txt**

```
Apple
Mango
Orange
```

Output:

```
  Apple
- Banana
+ Mango
  Orange
```

---

### Missing Arguments

```
diff file1.txt
```

Output:

```
diff: missing file operands
```

---

### File Not Found

```
diff file1.txt file2.txt
```

Output:

```
diff: cannot open file2.txt
```

---

## Output Symbols

| Prefix | Meaning |
|---------|---------|
| `  ` | Line exists in both files. |
| `-` | Line removed from the first file. |
| `+` | Line added in the second file. |

---

## C++ Features Used

- `std::vector`
- `std::ifstream`
- `std::getline()`
- Dynamic Programming (LCS)
- `std::max()`
- `std::reverse()`
- Exception handling (`try-catch`)
- `std::cout`
- `std::cerr`

---

## Limitations

- Compares files **line by line** rather than character by character.
- Uses the Longest Common Subsequence algorithm, requiring **O(n × m)** memory.
- May consume significant memory for very large files.
- Does not support Linux `diff` options such as unified (`-u`), context (`-c`), side-by-side (`-y`), or recursive directory comparison.
- Displays a simplified diff format using only `+`, `-`, and unchanged line markers.

---

## Summary

The `diff` command implementation compares two text files using the **Longest Common Subsequence (LCS)** algorithm. It reads both files into memory, constructs a dynamic programming table to identify the longest shared sequence of lines, backtracks to determine additions and deletions, and displays the differences using intuitive line prefixes. This approach provides an efficient and educational implementation of a fundamental file comparison utility for the custom terminal application.