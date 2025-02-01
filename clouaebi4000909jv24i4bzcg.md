---
title: "Learning awk"
seoTitle: "Mastering AWK in Linux: Essential Guide for Text Processing"
seoDescription: "Unlock the power of AWK in Linux for text processing. Learn field separators, pattern matching, and data extraction in this concise, expert guide"
datePublished: Sat Nov 11 2023 16:54:31 GMT+0000 (Coordinated Universal Time)
cuid: clouaebi4000909jv24i4bzcg
slug: learning-awk
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/xbEVM6oJ1Fs/upload/faed30dd4cc198f2a7de5b6f70675c1f.jpeg
tags: command-line, linux-for-beginners

---

## **What is AWK?**

You can check my latest articles on [https://learncodecamp.net/](https://learncodecamp.net/)

Awk is a programming language designed for text processing. At its core, AWK is used for scanning and processing patterns in files and then taking action based on those patterns.

## **Basic Syntax of AWK**

The essential syntax of an `awk` command is `awk [options] 'program' input-file(s)`. Here, the 'program' consists of a series of 'patterns' and corresponding 'actions'. The action is enclosed in `{}` and is performed when the pattern matches.

## **Getting Started with AWK**

### **Printing Lines and Fields**

To print all lines from a file, use:

```bash
awk '{print}' filename
```

To print specific fields (e.g., first and second), the command is:

```bash
awk '{print $1, $2}' filename
```

### **Handling Field Separators**

By default, AWK treats spaces and tabs as field separators. To specify a tab as a separator, use:

```bash
awk -F '\t' '{print $1, $2}' filename
```

### **Pattern Matching**

AWK excels in pattern matching:

```bash
awk '/pattern/ {print $0}' filename
```

For case-insensitive matching, use `tolower`:

```bash
awk 'tolower($0) ~ /pattern/ {print $0}' filename
```

Let's try to parse `ls -ltr` command output with awk, and try to print just the filenames.

The challenge arises when you encounter filenames with spaces. A simple `awk` command may not correctly handle such cases. Here's a more nuanced approach:

```bash
ls -ltr | awk '{for (i=9; i<=NF; i++) printf "%s%s", $i, (i==NF ? "\n" : " ")}'
```

### **Built-in Variables**

* `$0`: The entire current line.
    
* `$1`, `$2`, ...: The first, second, etc., fields of the current line.
    
* `NR`: Number of records (typically lines) processed so far.
    
* `NF`: Number of fields in the current record.
    

this command works as follows:

* It loops from the 9th field (which is where filenames start in the `ls -ltr` output) to the last field (`NF`).
    
* It prints each of these fields, adding a space between them unless it's the last field, in which case it adds a newline.
    
* This effectively reconstructs filenames with spaces.
    

### **More Complex Operations**

* **Summing a Column**:
    
    ```bash
    awk '{sum += $1} END {print sum}' filename
    ```
    
    This sums up the values in the first field of each line.
    
* **Text Processing**:
    
    ```bash
    awk '/pattern/ {gsub(/old/, "new"); print}' filename
    ```
    
    This finds lines matching `pattern`, replaces 'old' with 'new' in them, and prints the result.
    

### **Options in AWK**

The `options` in `awk` are command-line flags that modify its behavior. Some common options include:

1. `-F`: Specifies a field separator.
    
    * Example: `-F ':'` uses a colon as the field separator.
        
2. `-v`: Allows you to set a variable.
    
    * Example: `-v OFS=','` sets the output field separator to a comma.
        
3. `-f`: Specifies that the `awk` program is to be read from a file.
    
    * Example: `awk -f program.awk inputfile` reads the `awk` program from `program.awk`.
        

These options can be used to alter how `awk` processes input files and handles data.

### BEGIN

The `BEGIN` keyword in `awk` is used to specify actions that should be executed before any input lines are read. It's a special kind of pattern which does not process input text but sets up initial conditions or configurations for the `awk` program. The `BEGIN` block is executed only once, at the very start of the program, making it an ideal place for initialization tasks.

While we've covered the basic usage of `awk`, delving into some of its internal variables will significantly enhance your ability to manipulate and format text data efficiently. These variables include `FS`, `RS`, `OFS`, and `ORS`.

### **Field Separator (FS)**

* **What it does**: `FS` stands for Field Separator. It's the character that `awk` uses to divide fields on an input line.
    
* **Default Value**: By default, `FS` is set to white space, meaning any space or tab character.
    
* **Customization**: You can change `FS` to a different character, like a comma or colon, to match the structure of your input data. This is often done in the `BEGIN` block of an `awk` program.
    
    ```bash
    awk 'BEGIN {FS=":"} {print $1}' filename
    ```
    

### **Record Separator (RS)**

* **What it does**: `RS` is the Record Separator. It defines the end of a record.
    
* **Default Value**: The default `RS` is a newline character, meaning each line in the input is treated as a separate record.
    
* **Usage**: Changing `RS` can be useful for processing data where records are separated by different characters or patterns.
    

### **Output Field Separator (OFS)**

* **What it does**: `OFS` stands for Output Field Separator. It's used by `awk` to separate fields when printing output.
    
* **Default Value**: The default `OFS` is a space.
    
* **Functionality**: If your `awk` print statement contains multiple fields separated by commas, `awk` will insert the `OFS` value between them.
    
    ```bash
    awk 'BEGIN {OFS=","} {print $1, $2}' filename
    ```
    

### **Output Record Separator (ORS)**

* **What it does**: `ORS` is the Output Record Separator, dictating how `awk` separates output records.
    
* **Default Value**: The default for `ORS` is a newline character, meaning each print statement results in a new line.
    
* **Implications**: By altering `ORS`, you can change how the output is formatted in terms of record separation.
    

AWK is a powerful tool that can greatly simplify text processing tasks in Linux. Whether it's extracting columns from a file or parsing complex command outputs.

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.