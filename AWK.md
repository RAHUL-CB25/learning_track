# AWK Basics — Detailed Learning Report

**Prepared by:** Rahul Raj
**Track:** Linux  — AWK Fundamentals 

---

## Overview


```
awk 'BEGIN{ ... } pattern { action } END{ ... }' filename
```

- **BEGIN{ }** → runs once, *before* any line of the file is read (used to set variables like `FS`, `OFS`, `RS`, `ORS`, or print headers).
- **pattern { action }** → runs the `{ action }` for *every line* that matches `pattern`. If no pattern is given, the action runs on every line.
- **END{ }** → runs once, *after* all lines have been read (used to print totals, summaries, footers).

Each of the 16 topics below includes: a **definition**, the **sample data** used, the **exact command**, a **word-by-word breakdown** of that command, and the **output** produced.

---

---

## 1. Introduction to AWK

**Definition:** AWK is a pattern-scanning and text-processing language created for manipulating structured text (rows and columns). It reads input **line by line**, automatically splits each line into **fields** (`$1`, `$2`, `$3`, ...), and lets you run an **action** on lines that match a **pattern**. `$0` always refers to the *whole line*.

**Data (`file1.txt`):**
```
1) Amit     Physics   80
2) Rahul    Maths     90
3) Shyam    Biology   87
4) Kedar    English   85
5) Hari     History   89
```

**Command:**
```bash
awk '{print $1,$2}' file1.txt
```

**Word-by-word breakdown:**
| Part | Meaning |
|---|---|
| `awk` | Calls the AWK interpreter/program. |
| `'{print $1,$2}'` | The AWK program itself, wrapped in single quotes so the shell doesn't interpret `$` as a shell variable. |
| `{ ... }` | Since no pattern is specified before it, this action applies to **every line**. |
| `print` | The AWK built-in command that outputs values to the screen. |
| `$1` | The **1st field** of the current line (split by whitespace by default). |
| `,` | Comma between `$1` and `$2` tells AWK to print both, separated by a single space (the default `OFS`). |
| `$2` | The **2nd field** of the current line. |
| `file1.txt` | The input file AWK reads and processes line by line. |

**Output:**
```
1) Amit
2) Rahul
3) Shyam
4) Kedar
5) Hari
```

---

## 2. Executing AWK in Different Ways

**Definition:** AWK programs can be run in three common ways: (a) typed directly on the command line, (b) saved in a `.awk` script file and run with `-f`, or (c) fed data through a pipe from another command.

**Data (`file2.txt`):**
```
101 Rahul Developer 55000 Bangalore
102 Priya Tester 48000 Chennai
103 Aman Manager 75000 Mumbai
104 Neha HR 45000 Delhi
105 Karan DevOps 68000 Hyderabad
```

**Command 1 — Inline program:**
```bash
awk '{print $2}' file2.txt
```
| Part | Meaning |
|---|---|
| `awk` | Invokes AWK. |
| `'{print $2}'` | Inline program: for every line, print field 2. |
| `file2.txt` | File passed as the input source. |

**Output:**
```
Rahul
Priya
Aman
Neha
Karan
```

**Command 2 — Script file:**
```bash
awk -f test1.awk file2.txt
```
| Part | Meaning |
|---|---|
| `awk` | Invokes AWK. |
| `-f` | Flag telling AWK: "read the program from a **file**, not from the command line." |
| `test1.awk` | The script file containing the AWK program (with `BEGIN`, main block, `END`). |
| `file2.txt` | The data file the script is applied to. |

**Output (script prints a header and footer around the names):**
```
hello header
Rahul
Priya
Aman
Neha
Karan
ending
```

**Command 3 — Piped input:**
```bash
cat file2.txt | awk '{print $3}'
```
| Part | Meaning |
|---|---|
| `cat file2.txt` | Reads and streams the file's contents to standard output. |
| `\|` | Pipe operator — sends the output of `cat` as the **input** to the next command. |
| `awk '{print $3}'` | AWK reads from standard input (since no filename is given) and prints field 3 of each line it receives. |

**Output:**
```
Developer
Tester
Manager
HR
DevOps
```

---

## 3. Customizing Input Field Separator (FS)

**Definition:** `FS` (Field Separator) tells AWK **how to split each input line into fields**. By default `FS` is any amount of whitespace (spaces/tabs). It can be changed to any single character or regex, such as `:`, `,`, or `|`.

**Data (`file5.txt`):**
```
apple 100 available:Mango 50 NotAvailable:Strawberry 100 NA
Orange 80 available:Grapes 120 Available:Banana 60 NA
Pineapple 150 Available:Kiwi 200 NotAvailable:Papaya 90 Available
Tomato 40 Available:Potato 30 Available:Onion 70 NotAvailable
Milk 60 Available:Bread 40 Available:Butter 120 Available
```

**Command:**
```bash
awk 'BEGIN{FS=":"}{print $1}' file5.txt
```

**Word-by-word breakdown:**
| Part | Meaning |
|---|---|
| `awk` | Invokes AWK. |
| `BEGIN{ ... }` | Block that executes **once**, before reading any line — the right place to set `FS` since it must be set before fields are split. |
| `FS=":"` | Assigns the field separator variable `FS` the value `":"` (colon). Every line will now be split wherever a colon appears. |
| `{print $1}` | Main action block (runs for every line): print the 1st field, which is now everything **before the first colon**. |
| `file5.txt` | Input file. |

**Output:**
```
apple 100 available
Orange 80 available
Pineapple 150 Available
Tomato 40 Available
Milk 60 Available
```

---

## 4. Customizing Output Field Separator (OFS)

**Definition:** `OFS` (Output Field Separator) controls the character **placed between fields** when multiple fields are printed together using a comma inside `print`. Default `OFS` is a single space.

**Data (`file2.txt`)** — same as Topic 2.

**Command:**
```bash
awk 'BEGIN{OFS="-"}{print $2,$3,$4}' file2.txt
```

**Word-by-word breakdown:**
| Part | Meaning |
|---|---|
| `BEGIN{OFS="-"}` | Before processing starts, set the output field separator to a hyphen `-` instead of the default space. |
| `{print $2,$3,$4}` | For every line, print field 2, field 3, and field 4. Because they're separated by commas in the `print` statement, AWK inserts `OFS` (now `-`) between them in the output. |
| `file2.txt` | Input file. |

**Output:**
```
Rahul-Developer-55000
Priya-Tester-48000
Aman-Manager-75000
Neha-HR-45000
Karan-DevOps-68000
```

---

## 5. Customizing Input Record Separator (RS)

**Definition:** `RS` (Record Separator) defines what AWK treats as the boundary between one **record** (normally a "line") and the next. Default `RS` is a newline (`\n`). Changing it lets you treat other characters — like `|` — as line breaks.

**Data (`file6.txt`):**
```
John,Manager,12000|Mathew,IT Lead,11000|Praveen,Architect,13000|Rahul,Developer,55000|Priya,Tester,48000|Aman,Manager,75000|Neha,HR,45000|Karan,DevOps,68000
```

**Command:**
```bash
awk 'BEGIN{RS="|"; FS=","} {print $1}' file6.txt
```

**Word-by-word breakdown:**
| Part | Meaning |
|---|---|
| `BEGIN{ ... }` | Runs once before reading, to configure separators. |
| `RS="\|"` | Sets the record separator to the pipe character. AWK will now treat everything between two `\|` characters as one record (instead of one line). |
| `;` | Statement separator inside the same `BEGIN` block — lets you set multiple variables in one line. |
| `FS=","` | Within each record, split fields by comma. |
| `{print $1}` | For every record, print field 1 — the person's name. |
| `file6.txt` | Input file. |

**Output:**
```
John
Mathew
Praveen
Rahul
Priya
Aman
Neha
Karan
```

> **Environment note:** The default `mawk` interpreter on this system handled `RS="|"` inconsistently. Installing **GNU AWK (`gawk`)** via `sudo apt install gawk` gave correct, consistent record-splitting. `sudo apt update` refreshed the package lists first, then `sudo apt install gawk` downloaded and installed the `gawk` package (527 kB) alongside its dependencies.

---

## 6. Customizing Output Record Separator (ORS)

**Definition:** `ORS` (Output Record Separator) controls what character AWK prints **after each output record**, replacing the default newline (`\n`). Useful for joining multiple records onto one line, or using a custom delimiter.

**Data (`file1.txt`)** — same as Topic 1.

**Command 1:**
```bash
awk 'BEGIN{ORS=","} {print $1,$2}' file1.txt
```
| Part | Meaning |
|---|---|
| `BEGIN{ORS=","}` | Before processing, set the output record separator to a comma instead of a newline. |
| `{print $1,$2}` | For each line, print field 1 and field 2 (separated by the default `OFS`, a space). |
| End of each `print` | Normally prints `\n`; now prints `,` instead — so output lines run together, comma-separated. |

**Output:**
```
1) Amit,2) Rahul,3) Shyam,4) Kedar,5) Hari,
```

**Command 2 (custom multi-character ORS):**
```bash
awk 'BEGIN{ORS="---"} {print $1,$2}' file1.txt
```
| Part | Meaning |
|---|---|
| `ORS="---"` | `ORS` can be a **string of multiple characters**, not just one — here three hyphens are used as the separator after each record. |

**Output:**
```
1) Amit---2) Rahul---3) Shyam---4) Kedar---5) Hari---
```

**Command 3 (FS + RS + ORS combined, `file6.txt`):**
```bash
awk 'BEGIN{FS=","; RS="|"; ORS="+"} {print $1,$2}' file6.txt
```
| Part | Meaning |
|---|---|
| `FS=","` | Split each record's fields by comma. |
| `RS="\|"` | Treat `\|` as the boundary between records. |
| `ORS="+"` | Print `+` after each output record instead of a newline. |
| `{print $1,$2}` | For each record, print the name (`$1`) and role (`$2`). |

**Output:**
```
John Manager+Mathew IT Lead+Praveen Architect+Rahul Developer+Priya Tester+Aman Manager+Neha HR+Karan DevOps+
```

---

## 7. Getting Number of Fields and Records (NF & NR)

**Definition:**
- **`NF`** = "Number of Fields" — a built-in variable holding the count of fields in the *current* line.
- **`NR`** = "Number of Records" — a built-in variable holding the count (index) of the *current* line being processed, starting at 1.

**Data (`file2.txt`)** — same as Topic 2.

**Command 1:**
```bash
awk '{print NF}' file2.txt
```
| Part | Meaning |
|---|---|
| `{print NF}` | For every line, print how many fields it was split into (here: 5 fields per line — id, name, role, salary, city). |

**Output:**
```
5
5
5
5
5
```

**Command 2:**
```bash
awk '{print NR}' file2.txt
```
| Part | Meaning |
|---|---|
| `{print NR}` | For every line, print its record number — 1 for the first line, 2 for the second, and so on. |

**Output:**
```
1
2
3
4
5
```

---

## 8. Regular Expressions in AWK

**Definition:** AWK supports regex-based matching. A pattern written as `/regex/` matches any line where the *whole line* contains that pattern. The `~` operator means "matches", and `!~` means "does not match" — both can be applied to specific fields, not just whole lines.

**Data (`file2.txt`)** — same as Topic 2.

**Command 1:**
```bash
awk '/Rahul/ {print $2}' file2.txt
```
| Part | Meaning |
|---|---|
| `/Rahul/` | Pattern: matches any line containing the literal text "Rahul" anywhere in it. |
| `{print $2}` | Action, applied only to matching lines: print field 2. |

**Output:**
```
Rahul
```

**Command 2:**
```bash
awk '/i$/ {print $1,$2,$3,$4,$5}' file2.txt
```
| Part | Meaning |
|---|---|
| `/i$/` | Regex: `i` followed by `$` (end-of-line anchor) — matches lines that **end with the letter "i"** (e.g., Chennai, Mumbai, Delhi). |
| `{print $1,$2,$3,$4,$5}` | Print all 5 fields of each matching line. |

**Output:**
```
102 Priya Tester 48000 Chennai
103 Aman Manager 75000 Mumbai
104 Neha HR 45000 Delhi
```

**Command 3:**
```bash
awk '$2 ~ /^[RK]/ {print $2}' file2.txt
```
| Part | Meaning |
|---|---|
| `$2` | Refers specifically to field 2 (the name), not the whole line. |
| `~` | The "matches" operator — tests if `$2` matches the regex that follows. |
| `/^[RK]/` | Regex: `^` anchors to the **start** of the field, `[RK]` is a character class matching either `R` or `K` — so it matches names starting with R or K. |
| `{print $2}` | Print field 2 for lines where the condition is true. |

**Output:**
```
Rahul
Karan
```

**Data (`file3.txt`):**
```
colour
coloe
color
```

**Command (`?` = optional character):**
```bash
awk '/colou?r/' file3.txt
```
| Part | Meaning |
|---|---|
| `/colou?r/` | Regex: the `u?` means the letter **"u" is optional** (0 or 1 occurrence) — matches both "colour" (British) and "color" (American spelling). |
| *(no `{action}` given)* | When only a pattern is given with no explicit action, AWK's default action is `{print $0}` — print the whole matching line. |

**Output:**
```
colour
color
```

**Data (`file2.txt` — Call/Tall/Ball style):**
```
Call
Tall
Ball
```

**Command (character class):**
```bash
awk '/[CT]all/' file2.txt
```
| Part | Meaning |
|---|---|
| `[CT]` | Character class — matches a single character that is either `C` or `T`. |
| `all` | Must be followed literally by "all". |
| `/[CT]all/` | Overall: matches "Call" or "Tall", but not "Ball" (since `B` isn't in the class). |

**Output:**
```
Call
Tall
```

---

## 9. Using Arithmetic Operators in AWK — Part 1

**Definition:** AWK supports standard arithmetic operators directly inside expressions: `+` (add), `-` (subtract), `*` (multiply), `/` (divide), `%` (modulus/remainder), and `^` (exponent/power).

**Data (`file2.txt`)** — same as Topic 2 (salary is field 4).

**Command (calculate annual salary):**
```bash
awk '{print $2, $4*12}' file2.txt
```

**Word-by-word breakdown:**
| Part | Meaning |
|---|---|
| `{print $2, $4*12}` | Action block: print field 2 (name), then a value computed as field 4 (monthly salary) multiplied by 12 (months in a year). |
| `$4*12` | `$4` = monthly salary field; `*` = multiplication operator; `12` = literal number. AWK computes this before printing. |

**Output:**
```
Rahul 660000
Priya 576000
Aman 900000
Neha 540000
Karan 816000
```

---

## 10. Using Arithmetic Operators in AWK — Part 2

**Definition:** Continuation of arithmetic operators — showing how multiple operators combine in a single expression, and how parentheses `()` control order of evaluation (precedence), just like in normal math.

**Data (`file2.txt`)** — same as Topic 2.

**Command (10% tax deduction):**
```bash
awk '{print $2, $4 - ($4*10/100)}' file2.txt
```

**Word-by-word breakdown:**
| Part | Meaning |
|---|---|
| `$4` | Monthly salary (before deduction). |
| `- (...)` | Subtracts whatever is computed inside the parentheses from `$4`. |
| `$4*10/100` | Computes 10% of the salary: multiply salary by 10, then divide by 100 — parentheses force this to be calculated **first**, before the subtraction. |
| `print $2, ...` | Print the employee's name alongside their salary after deduction. |

**Output:**
```
Rahul 49500
Priya 43200
Aman 67500
Neha 40500
Karan 61200
```

---

## 11. Using Increment Operators in AWK

**Definition:** AWK supports `++` (increment by 1) and `--` (decrement by 1). Written as `i++` (post-increment: use the value, then increase it) or `++i` (pre-increment: increase first, then use it). Commonly used for counters inside loops or `END` blocks.

**Data (`file2.txt`)** — same as Topic 2.

**Command (row counter):**
```bash
awk '{count++} END{print "Total records:", count}' file2.txt
```

**Word-by-word breakdown:**
| Part | Meaning |
|---|---|
| `{count++}` | For every line processed, increase the variable `count` by 1. (`count` starts at 0 automatically the first time it's used — AWK auto-initializes numeric variables.) |
| `count++` | Post-increment shorthand for `count = count + 1`. |
| `END{ ... }` | Runs once, after all lines are read. |
| `print "Total records:", count` | Prints the literal label `"Total records:"` followed by the final value stored in `count`. |

**Output:**
```
Total records: 5
```

---

## 12. AWK Sample Use Case

**Definition:** A practical, real-world example combining field access, an **associative array**, and a `for...in` loop to summarize data — here, counting how many employees hold each role.

**Data (`file2.txt`)** — same as Topic 2.

**Command:**
```bash
awk '{roles[$3]++} END{for (r in roles) print r, roles[r]}' file2.txt
```

**Word-by-word breakdown:**
| Part | Meaning |
|---|---|
| `roles[$3]` | Creates/accesses an **associative array** called `roles`, using the value of field 3 (job role, e.g., "Developer") as the **key**. |
| `roles[$3]++` | Increments the counter stored under that role's key each time it's seen — tallying occurrences per role. |
| `END{ ... }` | Runs after all lines have been processed, once every role has been counted. |
| `for (r in roles)` | A loop that iterates over every **key** (`r`) currently stored in the `roles` array. |
| `print r, roles[r]` | Prints the role name (`r`) and its count (`roles[r]`). |

**Output:**
```
Developer 1
Tester 1
Manager 1
HR 1
DevOps 1
```

---

## 13. Using if-else in AWK

**Definition:** AWK supports standard conditional logic with `if (condition) { ... } else { ... }`, allowing different actions depending on whether a condition is true or false.

**Data (`file2.txt`)** — same as Topic 2 (salary in field 4).

**Command:**
```bash
awk '{ if ($4 > 50000) print $2, "High Salary"; else print $2, "Low Salary" }' file2.txt
```

**Word-by-word breakdown:**
| Part | Meaning |
|---|---|
| `if ($4 > 50000)` | Condition: checks if field 4 (salary) is greater than 50000. |
| `print $2, "High Salary"` | Runs **only if the condition is true** — prints the name and the label "High Salary". |
| `;` | Statement separator, ending the `if` branch before starting the `else` branch. |
| `else` | Runs the following statement **only if the condition was false**. |
| `print $2, "Low Salary"` | Prints the name with the label "Low Salary" when salary is 50000 or below. |

**Output:**
```
Rahul High Salary
Priya Low Salary
Aman High Salary
Neha Low Salary
Karan High Salary
```

---

## 14. Using if-else if in AWK

**Definition:** Chaining `else if` after an `if` allows classification into **more than two** categories, testing conditions in order until one is true.

**Script (`case2.awk`):**
```awk
BEGIN{
    FS=","
    infant=0
    adult=0
    senior=0
}
{
    if ( $5 < 18 )
    {
        infant++
    }
    else if ( $5 > 18 && $5 < 61 )
    {
        adult++
    }
    else
    {
        senior++
    }
}
END {
    print "Number of infants" , infant
    print "Number of adults", adult
    print "Number of senior" , senior
}
```

**Word-by-word breakdown:**
| Part | Meaning |
|---|---|
| `BEGIN{ FS=","; infant=0; adult=0; senior=0 }` | Before reading data: set the field separator to comma, and initialize three counters to zero. |
| `if ( $5 < 18 )` | Checks if field 5 (age) is less than 18. |
| `infant++` | If true, increment the `infant` counter. |
| `else if ( $5 > 18 && $5 < 61 )` | If the first condition was false, check this one: age is greater than 18 **AND** (`&&`) less than 61. |
| `adult++` | If this condition is true, increment the `adult` counter. |
| `else { senior++ }` | If neither condition matched (age is 61 or above), increment the `senior` counter. |
| `END{ print ... }` | After all records are processed, print the three final totals with descriptive labels. |

**Data:** CSV file with columns `id, first_name, last_name, email, gender, location` (age is the 5th field in the working dataset used for this exercise).

**Command:**
```bash
awk -f case2.awk file1.txt
```
| Part | Meaning |
|---|---|
| `-f case2.awk` | Load the program from the script file `case2.awk`. |
| `file1.txt` | The data file to process. |

**Output (sample):**
```
Number of infants 5
Number of adults 22
Number of senior 8
```

---

## 15. While Loop in AWK

**Definition:** A `while (condition) { ... }` loop repeats the block inside it **as long as the condition remains true** — useful for iterating over every field in a line without knowing `NF` in advance.

**Script (`solution.awk`):**
```awk
i=1
sum=0
while ( i <= NF )
{
    sum = sum + $i
    i++
}
print "Sum is ", sum
```

**Word-by-word breakdown:**
| Part | Meaning |
|---|---|
| `i=1` | Initialize a counter variable `i` to 1 (this runs for every line, since it's outside `BEGIN`). |
| `sum=0` | Initialize an accumulator variable `sum` to 0. |
| `while ( i <= NF )` | Loop condition: keep looping as long as `i` is less than or equal to the number of fields in the current line. |
| `sum = sum + $i` | Inside the loop: add the value of the field at position `i` (e.g., `$1`, then `$2`, etc.) to the running total `sum`. |
| `i++` | Increment `i` by 1 so the loop eventually reaches every field and then stops. |
| `print "Sum is ", sum` | After the loop ends for that line, print the label and the total. |

**Data (`input.txt`):**
```
10 24 56 50 2
100 200 300
10 10 10 10 10 10
20 10 10 1 1 1
100 100 100 100 100 100 100
```

**Command:**
```bash
awk -f solution.awk input.txt
```
| Part | Meaning |
|---|---|
| `-f solution.awk` | Run the program stored in `solution.awk`. |
| `input.txt` | Each line of numbers is summed independently. |

**Output:**
```
Sum is  142
Sum is  600
Sum is  60
Sum is  43
Sum is  700
```

---

---

## Summary

Over these 16 topics, I practiced and can now explain, word by word, how each of the following works:

| Concept | What it controls |
|---|---|
| `BEGIN{ }` / `END{ }` | Code that runs once before/after all data is processed |
| `FS` | How input lines are split into fields |
| `OFS` | The separator placed between fields on output |
| `RS` | What counts as one "record" in the input |
| `ORS` | The separator placed after each output record |
| `NF` / `NR` | Number of fields in current line / current line number |
| `$0`, `$1`...`$NF` | Whole line / individual fields, including the dynamic "last field" |
| `/regex/`, `~`, `!~` | Pattern matching on whole lines or specific fields |
| `+ - * / % ^` | Arithmetic operators |
| `++` / `--` | Increment/decrement operators |
| `array[key]++`, `for (x in array)` | Associative arrays and iterating over them |
| `if / else if / else` | Conditional branching |
| `while ( condition ) { }` | Repeating an action while a condition holds |
| `function name() { }` | Defining and calling reusable custom functions |

---

## References

- GNU Awk User's Guide — Free Software Foundation. https://www.gnu.org/software/gawk/manual/gawk.html
- *The AWK Programming Language* — A. Aho, B. Kernighan, P. Weinberger (Addison-Wesley).
- AWK command in Linux/Unix with examples — GeeksforGeeks. https://www.geeksforgeeks.org/awk-command-unixlinux-examples/
- AWK Tutorial — Tutorialspoint. https://www.tutorialspoint.com/awk/index.htm
- Regular Expressions in awk — GNU Awk User's Guide, chapter on regexp. https://www.gnu.org/software/gawk/manual/html_node/Regexp.html
- `man awk` / `man gawk` — local manual pages (run on Ubuntu 22.04, gawk package).
- Stack Overflow — various threads referenced for RS/FS edge cases with mawk vs gawk. https://stackoverflow.com/questions/tagged/awk
