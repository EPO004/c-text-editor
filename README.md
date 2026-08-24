# C Text Editor — Vim-Inspired Command-Line Editor

A command-line text editor and file manipulation tool written from scratch in **C**, inspired by the command-oriented workflow of editors such as **Vim**.

This project was developed as a **Fundamentals of Programming** course project. It implements common text-editing, searching, file-management, and formatting operations directly in C.

The goal of the project was to practice low-level programming concepts such as file I/O, dynamic memory management, string processing, command parsing, filesystem traversal, and implementation of text-editing operations.

---

## Features

The editor provides a collection of commands for manipulating text files from the terminal.

### File Operations

- Create files and directories
- Display file contents
- Work with nested directory structures
- Display directory trees
- Compare two text files

### Text Editing

- Insert text at a specified position
- Remove text
- Copy text
- Cut text
- Paste text
- Undo editing operations

### Search

- Search for strings inside files
- Count occurrences
- Find specific occurrences
- Return all matches
- Search based on word position
- Basic wildcard support

### Search and Replace

- Replace the first occurrence
- Replace a specific occurrence
- Replace all occurrences

### Grep-like Search

Search across multiple files using functionality similar to the Unix `grep` command.

Supported modes include:

- Print matching lines
- Count matches
- Print filenames containing the pattern

### File Comparison

Compare two text files and identify differences between their contents.

### Directory Tree

Display the directory hierarchy recursively with a configurable depth.

### Code Formatting

The `organize` command reformats brace-based source code and adjusts indentation automatically.

---

## Technologies

- **C**
- Standard C file I/O
- Dynamic memory allocation
- String processing
- Directory traversal
- Custom command parser
- Windows filesystem APIs

---

## Project Structure

```text
FundamentalOfProgramming-Project/
│
├── v1.0.0.c
├── ver4.c
└── README.md
```

`v1.0.0.c` contains the main/latest implementation of the editor.

`ver4.c` is an earlier development version preserved in the repository.

---

## Building the Project

### Requirements

The current implementation is designed primarily for **Windows**.

You need:

- Windows
- GCC / MinGW-w64
- Git

The source currently uses Windows-specific headers and filesystem paths such as:

```text
E:\C\home
```

Therefore, the program may require modification before compiling on Linux or macOS.

### Clone the Repository

```bash
git clone https://github.com/EPO004/FundamentalOfProgramming-Project.git
cd FundamentalOfProgramming-Project
```

### Compile

Using GCC/MinGW:

```bash
gcc v1.0.0.c -o editor.exe
```

### Run

```bash
editor.exe
```

or from PowerShell:

```powershell
.\editor.exe
```

---

## Workspace

The current implementation uses the following base directory:

```text
E:\C\home
```

and some operations, such as the directory-tree command, work with:

```text
E:\C\home\root
```

Create these directories before running the program if they do not already exist.

For example:

```text
E:\
└── C\
    └── home\
        └── root\
```

Paths supplied to editor commands are interpreted relative to this workspace.

For example:

```text
/root/test.txt
```

corresponds to:

```text
E:\C\home\root\test.txt
```

---

# Command Examples

## Create a File

```text
createfile --file /root/example.txt
```

The command also creates missing directories in the supplied path.

---

## Insert Text

```text
insertstr --file /root/example.txt --str "Hello World" --pos 1:0
```

The position is represented as:

```text
line:character
```

For example:

```text
--pos 2:5
```

means line 2, character 5.

Escape sequences such as newline characters can also be processed by the command parser.

---

## Display a File

```text
cat --file /root/example.txt
```

This prints the contents of the file to the terminal.

---

## Remove Text

```text
removestr --file /root/example.txt --pos 1:5 -size 3 -f
```

Remove characters forward from the specified position.

To remove characters backward:

```text
removestr --file /root/example.txt --pos 1:5 -size 3 -b
```

---

## Copy Text

```text
copystr --file /root/example.txt --pos 1:0 -size 5 -f
```

The selected text is stored in the editor's internal clipboard.

---

## Cut Text

```text
cutstr --file /root/example.txt --pos 1:0 -size 5 -f
```

This copies the selected text to the clipboard and removes it from the file.

---

## Paste Text

```text
pastestr --file /root/example.txt --pos 1:10
```

The contents of the clipboard are inserted at the specified position.

---

## Find Text

```text
find --str "hello" --file /root/example.txt
```

### Count Matches

```text
find --str "hello" --file /root/example.txt -count
```

### Find All Occurrences

```text
find --str "hello" --file /root/example.txt -all
```

### Find a Specific Occurrence

```text
find --str "hello" --file /root/example.txt -at 2
```

### Word-Based Search

```text
find --str "hello" --file /root/example.txt -byword
```

Search functionality also includes basic wildcard handling.

---

## Replace Text

Replace the first occurrence:

```text
replace -s1 "hello" -s2 "world" --file /root/example.txt
```

Replace a specific occurrence:

```text
replace -s1 "hello" -s2 "world" --file /root/example.txt -at 2
```

Replace every occurrence:

```text
replace -s1 "hello" -s2 "world" --file /root/example.txt -all
```

---

## Grep

Search for text across several files:

```text
grep --str "hello" --files /root/file1.txt /root/file2.txt
```

Count matching occurrences:

```text
grep -c --str "hello" --files /root/file1.txt /root/file2.txt
```

Print names of files containing the string:

```text
grep -i --str "hello" --files /root/file1.txt /root/file2.txt
```

---

## Compare Files

```text
compare /root/file1.txt /root/file2.txt
```

The editor compares the contents of the two files and reports their differences.

---

## Undo

```text
undo --file /root/example.txt
```

Previous versions of edited files are temporarily stored so that modifications can be reverted.

---

## Directory Tree

Show the directory hierarchy:

```text
tree 3
```

where `3` specifies the maximum depth.

To display the complete tree:

```text
tree -1
```

---

## Organize / Auto Format

```text
organize /root/example.c
```

This command reformats brace-based source code and adjusts indentation.

For example, code such as:

```c
if (condition) {
printf("Hello");
if (another_condition) {
printf("World");
}
}
```

can be reorganized into a more structured format.

---

## Concepts Demonstrated

This project demonstrates several fundamental programming and systems concepts:

- File handling using `FILE *`
- Dynamic memory allocation using `malloc`, `calloc`, and `realloc`
- Pointer manipulation
- Character-level text processing
- String parsing
- Custom command-line parsing
- Filesystem traversal
- Directory management
- Clipboard implementation
- Undo mechanisms
- Pattern matching
- Search algorithms
- Text replacement algorithms
- Recursive directory traversal
- Basic source-code formatting

---

## How It Works

The application runs continuously and reads commands from standard input.

Conceptually, the main loop works as follows:

```text
                  ┌──────────────┐
                  │ Read Command │
                  └──────┬───────┘
                         │
                         ▼
                ┌──────────────────┐
                │ Parse Arguments  │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │ Identify Command │
                └────────┬─────────┘
                         │
          ┌──────────────┼──────────────┐
          │              │              │
          ▼              ▼              ▼
     File Editing      Search        Filesystem
       Commands        Commands       Commands
          │              │              │
          └──────────────┼──────────────┘
                         │
                         ▼
                 ┌───────────────┐
                 │ Update / Read │
                 │     File      │
                 └───────────────┘
```

The editor performs the requested operation directly on the corresponding files.

---

## Design

Unlike a full graphical or terminal-screen editor, this project uses a **command-driven interface**.

For example, instead of opening a file and moving a cursor interactively, an edit operation can be expressed directly:

```text
insertstr --file /root/test.txt --str "Hello" --pos 1:0
```

This design makes the project closer to a combination of:

- Vim-style command-driven editing
- Unix text-processing tools
- Basic filesystem utilities

implemented in C.

---

## Current Limitations

The current version was developed as an educational project and has several areas that could be improved:

- Windows-specific filesystem handling
- Hard-coded workspace paths
- Large monolithic source file
- Limited command-parser error handling
- No automated test suite
- No Makefile/CMake configuration
- No full-screen terminal user interface

These provide several directions for future development.

