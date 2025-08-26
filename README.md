# get_next_line

A C function that reads a text file line by line, regardless of the buffer size. This project teaches file I/O operations, dynamic memory allocation, and static variables management.

## 📋 Table of Contents

- [About](##about)
- [Function Overview](#function-overview)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Bonus Features](#bonus-features)
- [Testing](#testing)
- [Project Structure](#project-structure)
- [Implementation Details](#implementation-details)
- [License](#license)

## 🎯 About

get_next_line is a project from the 42 curriculum studied at 1337 School. The objective is to create a function that returns one line at a time from a text file, using a file descriptor. This project introduces concepts like static variables, buffer management, and efficient memory handling.

## ⚙️ Function Overview

### Mandatory Part

```c
char *get_next_line(int fd);
```

**Parameters:**
- `fd`: The file descriptor to read from

**Return Value:**
- The line that was read (including the `\n` if present)
- `NULL` if there's nothing more to read or if an error occurs

**Behavior:**
- Reads the file pointed to by the file descriptor one line at a time
- Each call returns the next line of the file
- Returns `NULL` when reaching EOF or on error
- Works with any buffer size defined at compilation

### Bonus Part

The bonus implementation handles **multiple file descriptors** simultaneously:

```c
char *get_next_line(int fd);
```

**Additional Features:**
- Can handle multiple file descriptors at the same time
- Maintains separate reading states for each file descriptor
- Allows reading from different files in alternating calls

## 🚀 Getting Started

### Prerequisites

- GCC compiler
- Make

### Compilation

The function can be compiled with different buffer sizes:

```bash
# Compile with default buffer size
gcc -Wall -Wextra -Werror -D BUFFER_SIZE=1024 get_next_line.c get_next_line_utils.c

# Compile with custom buffer size
gcc -Wall -Wextra -Werror -D BUFFER_SIZE=42 get_next_line.c get_next_line_utils.c
```

For bonus version:
```bash
gcc -Wall -Wextra -Werror -D BUFFER_SIZE=1024 get_next_line_bonus.c get_next_line_utils_bonus.c
```

### Installation

1. Clone the repository:
```bash
git clone https://github.com/abnemili/get_next_line_42.git
cd get_next_line_42
```

2. Include the header and source files in your project.

## 💻 Usage

Include the header in your C files:
```c
#include "get_next_line.h"
```

### Basic Usage Example

```c
#include "get_next_line.h"
#include <fcntl.h>
#include <stdio.h>

int main(void)
{
    int fd;
    char *line;

    fd = open("test.txt", O_RDONLY);
    if (fd == -1)
        return (1);

    while ((line = get_next_line(fd)) != NULL)
    {
        printf("%s", line);
        free(line);
    }
    
    close(fd);
    return (0);
}
```

### Bonus Usage Example (Multiple File Descriptors)

```c
#include "get_next_line_bonus.h"
#include <fcntl.h>
#include <stdio.h>

int main(void)
{
    int fd1, fd2;
    char *line;

    fd1 = open("file1.txt", O_RDONLY);
    fd2 = open("file2.txt", O_RDONLY);
    
    // Read alternately from both files
    line = get_next_line(fd1);
    printf("File1: %s", line);
    free(line);
    
    line = get_next_line(fd2);
    printf("File2: %s", line);
    free(line);
    
    line = get_next_line(fd1);
    printf("File1: %s", line);
    free(line);
    
    close(fd1);
    close(fd2);
    return (0);
}
```

## 🌟 Bonus Features

✅ **Multiple File Descriptor Support**
- Handle multiple files simultaneously
- Maintain separate static variables for each file descriptor
- Switch between different files without losing track of position

✅ **Memory Efficiency**
- Only one static variable per file descriptor
- Efficient buffer management
- Proper memory cleanup

## 🧪 Testing

### Manual Testing

Create test files with different characteristics:

```bash
# Test with different buffer sizes
gcc -D BUFFER_SIZE=1 get_next_line.c get_next_line_utils.c test.c -o test_1
gcc -D BUFFER_SIZE=1024 get_next_line.c get_next_line_utils.c test.c -o test_1024

# Test edge cases
echo -n "Line without newline" > test_no_newline.txt
echo -e "Line1\nLine2\nLine3" > test_normal.txt
echo "" > test_empty.txt
```

### Standard Input Testing

```c
#include "get_next_line.h"
#include <stdio.h>

int main(void)
{
    char *line;
    
    printf("Enter text (Ctrl+D to end):\n");
    while ((line = get_next_line(0)) != NULL)
    {
        printf("You entered: %s", line);
        free(line);
    }
    return (0);
}
```

### Bonus Testing

Test multiple file descriptors:
```c
// Test reading from 3 different files simultaneously
int fd1 = open("file1.txt", O_RDONLY);
int fd2 = open("file2.txt", O_RDONLY);
int fd3 = open("file3.txt", O_RDONLY);

// Read one line from each file in rotation
```

## 📁 Project Structure

### Mandatory Files
```
get_next_line_42/
├── get_next_line.h
├── get_next_line.c
├── get_next_line_utils.c
└── README.md
```

### Bonus Files
```
get_next_line_42/
├── get_next_line_bonus.h
├── get_next_line_bonus.c
├── get_next_line_utils_bonus.c
└── README.md
```

## 🔧 Implementation Details

### Key Challenges Solved

1. **Static Variables Management**: Using static variables to maintain reading state between function calls
2. **Dynamic Buffer Handling**: Managing variable buffer sizes efficiently
3. **Memory Management**: Proper allocation and deallocation of dynamic memory
4. **Line Termination**: Handling lines with and without newline characters
5. **Multiple File Descriptors**: (Bonus) Managing separate states for different file descriptors
6. **Edge Cases**: Empty files, large files, files without final newline

### Technical Approach

- **Buffer Reading**: Read chunks of BUFFER_SIZE bytes at a time
- **Line Assembly**: Concatenate chunks until a newline or EOF is found
- **Memory Efficiency**: Minimize memory allocations and avoid leaks
- **State Persistence**: Use static variables to remember reading position
- **Error Handling**: Robust handling of file errors and memory allocation failures

### Function Behavior

- **Return Value**: Always includes the `\n` character if present in the file
- **EOF Handling**: Returns the last line even if it doesn't end with `\n`
- **Error Cases**: Returns `NULL` for invalid file descriptor or memory allocation failure
- **Buffer Independence**: Works correctly with any BUFFER_SIZE (1, 1024, 10000, etc.)

## 🤝 Contributing

This is my personal implementation of the get_next_line project from the 42 curriculum at 1337 School. While the code is available for reference and learning purposes, please remember to follow 1337's academic integrity guidelines if you're a current student.

## 📄 License

This project was developed by me as part of the 42 curriculum studied at 1337 School. Feel free to explore and learn from the implementation!

---

**Note**: This is my personal implementation of get_next_line, including both mandatory and bonus parts. The function efficiently reads files line by line with support for multiple file descriptors and any buffer size.

*Crafted with 💻 by abnemili at 1337 School*
