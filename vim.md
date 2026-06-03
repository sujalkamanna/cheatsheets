# 📘 VimTutor Notes (Unofficial Learning Guide)

## 📌 Table of Contents

- [📘 VimTutor Notes (Unofficial Learning Guide)](#-vimtutor-notes-unofficial-learning-guide)
  - [📌 Table of Contents](#-table-of-contents)
  - [📎 Official Documentation](#-official-documentation)
  - [📎 Official Documentation, Attribution, Copyright \& Disclaimer](#-official-documentation-attribution-copyright--disclaimer)
    - [📖 Official Vim Documentation \& References](#-official-vim-documentation--references)
  - [🟢 Lesson 1 — Basics](#-lesson-1--basics)
    - [⌨️ 1.1 Move Cursor (h j k l)](#️-11-move-cursor-h-j-k-l)
    - [🚀 1.2 Start Vim / vimtutor](#-12-start-vim--vimtutor)
    - [🚪 1.3 Exit Vim (:q / :q! / :wq / ZZ)](#-13-exit-vim-q--q--wq--zz)
    - [❌ 1.4 Delete Character (x)](#-14-delete-character-x)
    - [✏️ 1.5 Insert Mode (i)](#️-15-insert-mode-i)
    - [➕ 1.6 Append Text (A)](#-16-append-text-a)
    - [🔁 1.7 Normal Mode (Esc)](#-17-normal-mode-esc)
    - [🎯 Lesson 1 Goal](#-lesson-1-goal)
    - [📌 Key Summary](#-key-summary)
- [🟡 Lesson 2 — Editing Basics](#-lesson-2--editing-basics)
  - [2.1 Delete Word (dw)](#21-delete-word-dw)
  - [2.2 Delete to End of Line (d$)](#22-delete-to-end-of-line-d)
  - [2.3 Operators and Motions](#23-operators-and-motions)
  - [2.4 Count + Motion (2w, 3w)](#24-count--motion-2w-3w)
  - [2.5 Count + Delete (d2w)](#25-count--delete-d2w)
  - [2.6 Delete Whole Lines (dd)](#26-delete-whole-lines-dd)
  - [2.7 Undo and Redo (u, U, Ctrl-r)](#27-undo-and-redo-u-u-ctrl-r)
  - [🎯 Lesson 2 Goal](#-lesson-2-goal)
  - [📌 Lesson 2 Summary](#-lesson-2-summary)
    - [Key Idea](#key-idea)
- [🔵 VimTutor — Lesson 3: Put, Replace, and Change](#-vimtutor--lesson-3-put-replace-and-change)
  - [3.1 The Put Command](#31-the-put-command)
    - [Example](#example)
    - [Useful Notes](#useful-notes)
  - [3.2 Replace a Character](#32-replace-a-character)
    - [Example](#example-1)
    - [Additional Examples](#additional-examples)
  - [3.3 The Change Command](#33-the-change-command)
    - [Example](#example-2)
  - [3.4 More Change Commands](#34-more-change-commands)
    - [Change to End of Word](#change-to-end-of-word)
    - [Change Word](#change-word)
    - [Change to End of Line](#change-to-end-of-line)
    - [Change Multiple Words](#change-multiple-words)
  - [🎯 Lesson 3 Goal](#-lesson-3-goal)
  - [📌 Lesson 3 Summary](#-lesson-3-summary)
  - [Key Idea](#key-idea-1)
- [🟣 VimTutor — Lesson 4: Search, Jump, and Substitute](#-vimtutor--lesson-4-search-jump-and-substitute)
  - [4.1 Cursor Location and File Status](#41-cursor-location-and-file-status)
    - [Navigate Through a File](#navigate-through-a-file)
    - [Example](#example-3)
  - [4.2 Search for Text](#42-search-for-text)
    - [Search Backward](#search-backward)
    - [Move Between Search Results](#move-between-search-results)
    - [Example](#example-4)
  - [4.3 Jump Between Matches](#43-jump-between-matches)
    - [Example Workflow](#example-workflow)
  - [4.4 Matching Parentheses and Brackets](#44-matching-parentheses-and-brackets)
    - [Example](#example-5)
    - [Another Example](#another-example)
  - [4.5 Substitute (Search and Replace)](#45-substitute-search-and-replace)
    - [Replace All Occurrences on the Current Line](#replace-all-occurrences-on-the-current-line)
    - [Example](#example-6)
  - [4.6 Replace in Multiple Lines](#46-replace-in-multiple-lines)
  - [4.7 Replace in Entire File](#47-replace-in-entire-file)
    - [Example](#example-7)
  - [4.8 Confirm Before Replacing](#48-confirm-before-replacing)
    - [Example Prompt](#example-prompt)
  - [🎯 Lesson 4 Goal](#-lesson-4-goal)
  - [📌 Lesson 4 Summary](#-lesson-4-summary)
  - [Key Commands to Remember](#key-commands-to-remember)
- [🔴 VimTutor — Lesson 5: External Commands and Files](#-vimtutor--lesson-5-external-commands-and-files)
  - [5.1 Run External Commands](#51-run-external-commands)
    - [Examples](#examples)
    - [Example Workflow](#example-workflow-1)
  - [5.2 Save to a File](#52-save-to-a-file)
    - [Save Under a Different Name](#save-under-a-different-name)
  - [5.3 Visual Selection + Save](#53-visual-selection--save)
    - [Save the Selection](#save-the-selection)
    - [Example](#example-8)
  - [5.4 Read a File into the Current Document](#54-read-a-file-into-the-current-document)
    - [Example](#example-9)
  - [5.5 Read Command Output into a Document](#55-read-command-output-into-a-document)
    - [Example](#example-10)
    - [Insert Current Date](#insert-current-date)
  - [Common Workflow Example](#common-workflow-example)
  - [🎯 Lesson 5 Goal](#-lesson-5-goal)
  - [📌 Lesson 5 Summary](#-lesson-5-summary)
  - [Key Commands to Remember](#key-commands-to-remember-1)
- [🟠 VimTutor — Lesson 6: Open, Copy, Replace, and Settings](#-vimtutor--lesson-6-open-copy-replace-and-settings)
  - [6.1 Open a New Line](#61-open-a-new-line)
    - [Example](#example-11)
    - [Open Above the Current Line](#open-above-the-current-line)
    - [Example](#example-12)
  - [6.2 Append Text](#62-append-text)
    - [Difference Between `i` and `a`](#difference-between-i-and-a)
    - [Example](#example-13)
    - [Example Using `A`](#example-using-a)
  - [6.3 Move to End of Word](#63-move-to-end-of-word)
    - [Example](#example-14)
    - [Useful Navigation Commands](#useful-navigation-commands)
  - [6.4 Yank (Copy) Text](#64-yank-copy-text)
    - [Examples](#examples-1)
  - [6.5 Paste Copied Text](#65-paste-copied-text)
    - [Duplicate a Line](#duplicate-a-line)
    - [Example Using `P`](#example-using-p)
  - [6.6 Replace Mode](#66-replace-mode)
    - [Example](#example-15)
    - [Exit Replace Mode](#exit-replace-mode)
    - [Difference Between `r` and `R`](#difference-between-r-and-r)
  - [6.7 Vim Settings](#67-vim-settings)
    - [Ignore Case During Search](#ignore-case-during-search)
    - [Incremental Search](#incremental-search)
    - [Highlight Search Results](#highlight-search-results)
    - [Disable an Option](#disable-an-option)
    - [Temporary vs Permanent Settings](#temporary-vs-permanent-settings)
  - [🎯 Lesson 6 Goal](#-lesson-6-goal)
  - [📌 Lesson 6 Summary](#-lesson-6-summary)
  - [Key Commands to Remember](#key-commands-to-remember-2)
- [⚫ VimTutor — Lesson 7: Help, Configuration, and Completion](#-vimtutor--lesson-7-help-configuration-and-completion)
  - [7.1 Open the Help System](#71-open-the-help-system)
    - [Why Vim Help Is Important](#why-vim-help-is-important)
  - [7.2 Get Help for a Specific Command](#72-get-help-for-a-specific-command)
    - [Examples](#examples-2)
  - [7.3 Move Between Windows](#73-move-between-windows)
    - [Example](#example-16)
    - [Useful Window Navigation](#useful-window-navigation)
  - [7.4 Close the Help Window](#74-close-the-help-window)
    - [Example Workflow](#example-workflow-2)
  - [7.5 Create a Vim Configuration File](#75-create-a-vim-configuration-file)
    - [Example Configuration](#example-configuration)
    - [Example `.vimrc`](#example-vimrc)
    - [Neovim Users](#neovim-users)
  - [7.6 Command Completion](#76-command-completion)
    - [Autocomplete with Tab](#autocomplete-with-tab)
    - [Completion Works For](#completion-works-for)
    - [Examples](#examples-3)
  - [Example Workflow](#example-workflow-3)
  - [🎯 Lesson 7 Goal](#-lesson-7-goal)
- [📌 Lesson 7 Summary](#-lesson-7-summary)
  - [Key Commands to Remember](#key-commands-to-remember-3)
  - [🎉 Congratulations!](#-congratulations)
    - [Recommended Next Topics](#recommended-next-topics)
  - [📎 Official Documentation, Attribution, Copyright \& Disclaimer](#-official-documentation-attribution-copyright--disclaimer-1)
    - [📖 Official Vim Documentation \& References](#-official-vim-documentation--references-1)
    - [📘 Nature of This Document](#-nature-of-this-document)
    - [⚖️ Copyright \& Ownership Notice](#️-copyright--ownership-notice)
    - [🚫 Non-Affiliation \& No Endorsement Statement](#-non-affiliation--no-endorsement-statement)
    - [📌 Usage Terms \& Responsibility Disclaimer](#-usage-terms--responsibility-disclaimer)
    - [🧠 Best Practice Recommendations](#-best-practice-recommendations)
    - [📜 Licensing Understanding (Informational Only)](#-licensing-understanding-informational-only)
    - [🧾 Final Statement](#-final-statement)

---
## 📎 Official Documentation

The following resources provide authoritative and up-to-date information about Vim:

* [Vim Help System (built-in `:help`)](https://vimhelp.org/)
* [Vim Reference Manual Index](https://vim-jp.org/vimdoc-en/vimindex.html)
* [Vim Documentation Overview](https://www.vim.org/docs.php)
* [Official Vim Website](https://www.vim.org/)

---

## 📎 Official Documentation, Attribution, Copyright & Disclaimer

### 📖 Official Vim Documentation & References

This document is based on publicly available official sources and standard Vim behavior. The primary references include:

* Vim Official Website: https://www.vim.org
* Built-in Vim Help System (`:help`)
* Vim User Manual: https://vimhelp.org
* Vim Reference Manual: https://vimhelp.org
* Ubuntu Documentation: https://help.ubuntu.com

...


---

## 🟢 Lesson 1 — Basics

Lesson 1 introduces the fundamental concepts of Vim, including how to move the cursor, enter and exit Vim, insert and delete text, and understand the basic operating modes of the editor.

These are the essential building blocks required for all future Vim usage.

---

### ⌨️ 1.1 Move Cursor (h j k l)

Vim uses keyboard-based navigation instead of relying on arrow keys.

Although arrow keys work in most modern Vim installations, learning `h`, `j`, `k`, and `l` is recommended because it keeps your hands on the keyboard's home row and makes navigation faster.

```text
h → move left
j → move down
k → move up
l → move right
```

This is one of the core habits that makes Vim efficient.

---

### 🚀 1.2 Start Vim / vimtutor

Open a file with Vim:

```bash
vim filename
```

Example:

```bash
vim notes.txt
```

Start the built-in Vim tutorial:

```bash
vimtutor
```

This launches a safe practice environment where you can learn Vim interactively without affecting your own files.

---

### 🚪 1.3 Exit Vim (:q / :q! / :wq / ZZ)

Quit Vim when no changes have been made:

```vim
:q
```

Quit without saving changes:

```vim
:q!
```

Save and quit:

```vim
:wq
```

Alternative save-and-quit shortcut:

```vim
ZZ
```

`ZZ` saves the file only if changes were made and then exits Vim.

Always ensure you are in **Normal mode** before using quit commands.

---

### ❌ 1.4 Delete Character (x)

Delete the character under the cursor:

```vim
x
```

Example:

Before:

```text
helo
   ^
```

Press:

```vim
x
```

Result:

```text
helo
```

The character under the cursor is removed.

---

### ✏️ 1.5 Insert Mode (i)

Enter Insert mode before the cursor:

```vim
i
```

Example:

Before:

```text
hello|
```

Press:

```vim
i
```

Type:

```text
 world
```

Result:

```text
hello world|
```

Press:

```text
Esc
```

to return to Normal mode.

---

### ➕ 1.6 Append Text (A)

Append text at the end of the current line:

```vim
A
```

This automatically moves the cursor to the end of the line and switches to Insert mode.

Example:

Before:

```text
Hello
```

Press:

```vim
A
```

Type:

```text
 World
```

Result:

```text
Hello World
```

---

### 🔁 1.7 Normal Mode (Esc)

Return to Normal mode:

```text
Esc
```

Normal mode is Vim's default mode.

Unlike most editors, Vim performs navigation, searching, copying, deleting, replacing, and many other operations from Normal mode.

Whenever something unexpected happens while learning Vim, press:

```text
Esc
```

This returns you to a known state so you can safely try the command again.

---

### 🎯 Lesson 1 Goal

By the end of Lesson 1, you should be able to:

- Open Vim
- Start VimTutor
- Move the cursor using `h`, `j`, `k`, and `l`
- Enter Insert mode
- Delete characters
- Return to Normal mode
- Save and quit Vim

---

### 📌 Key Summary

| Command | Action |
|----------|----------|
| `h` | Move left |
| `j` | Move down |
| `k` | Move up |
| `l` | Move right |
| `vim filename` | Open a file in Vim |
| `vimtutor` | Start the Vim tutorial |
| `:q` | Quit Vim |
| `:q!` | Quit without saving |
| `:wq` | Save and quit |
| `ZZ` | Save (if needed) and quit |
| `x` | Delete character |
| `i` | Enter Insert mode |
| `A` | Append at end of line |
| `Esc` | Return to Normal mode |

---

💡 Beginner Tip:

Whenever you feel stuck, confused, or unsure which mode you're in, press:

```text
Esc
```

You can safely press it multiple times. Vim will return to Normal mode, allowing you to start again.

---

# 🟡 Lesson 2 — Editing Basics

Lesson 2 introduces one of Vim's most important concepts:

**Operators + Motions**

Instead of performing a separate action for every edit, Vim allows you to combine commands to edit text quickly and efficiently.

Understanding this pattern is the foundation for becoming productive in Vim.

---

## 2.1 Delete Word (dw)

Move the cursor to the beginning of a word and type:

```vim
dw
```

Meaning:

* `d` = delete operator
* `w` = move to the next word

Together:

```text
dw = delete word
```

Example:

Before:

```text
There are a some words fun that don't belong paper in this sentence.
```

Use `dw` to remove the unnecessary words:

```text
a
fun
paper
```

After:

```text
There are some words that don't belong in this sentence.
```

---

## 2.2 Delete to End of Line (d$)

Place the cursor where unwanted text begins and type:

```vim
d$
```

Meaning:

* `$` = move to end of line
* `d$` = delete from cursor position to the end of the line

Example:

Before:

```text
Somebody typed the end of this line twice. end of this line twice.
```

Place the cursor after the first period and use:

```vim
d$
```

Result:

```text
Somebody typed the end of this line twice.
```

---

## 2.3 Operators and Motions

Many Vim editing commands follow a simple pattern:

```text
operator + motion
```

Examples:

| Command | Meaning               |
| ------- | --------------------- |
| `dw`    | Delete word           |
| `de`    | Delete to end of word |
| `d$`    | Delete to end of line |

Common motions:

| Motion | Meaning           |
| ------ | ----------------- |
| `w`    | Next word         |
| `e`    | End of word       |
| `$`    | End of line       |
| `0`    | Beginning of line |

Think of operators as actions and motions as destinations.

---

## 2.4 Count + Motion (2w, 3w)

You can place a number before a motion.

Example:

```vim
2w
```

Move forward two words.

More examples:

```vim
3w
```

Move forward three words.

```vim
5w
```

Move forward five words.

This is much faster than pressing `w` repeatedly.

---

## 2.5 Count + Delete (d2w)

Counts can also be combined with editing commands.

Example:

```vim
d2w
```

Meaning:

```text
delete + 2 words
```

Result:

The next two words are deleted.

Example:

Before:

```text
The quick brown fox jumps.
```

Cursor on:

```text
quick
```

Execute:

```vim
d2w
```

Result:

```text
The fox jumps.
```

---

## 2.6 Delete Whole Lines (dd)

Delete the current line:

```vim
dd
```

Delete two lines:

```vim
2dd
```

Delete three lines:

```vim
3dd
```

Example:

Before:

```text
Line 1
Line 2
Line 3
Line 4
```

Cursor on:

```text
Line 2
```

Execute:

```vim
dd
```

Result:

```text
Line 1
Line 3
Line 4
```

`dd` is one of the most frequently used Vim commands.

---

## 2.7 Undo and Redo (u, U, Ctrl-r)

Undo the most recent change:

```vim
u
```

Restore all changes made to the current line:

```vim
U
```

Redo a change that was undone:

```vim
Ctrl-r
```

Example workflow:

```vim
dd
u
Ctrl-r
```

Meaning:

* `dd` → delete line
* `u` → undo deletion
* `Ctrl-r` → redo deletion

This allows you to safely experiment while editing.

---

## 🎯 Lesson 2 Goal

By the end of Lesson 2, you should be able to:

* Delete words
* Delete portions of lines
* Delete complete lines
* Use counts with motions
* Combine operators and motions
* Undo mistakes
* Redo changes

---

## 📌 Lesson 2 Summary

| Command  | Action                    |
| -------- | ------------------------- |
| `dw`     | Delete word               |
| `de`     | Delete to end of word     |
| `d$`     | Delete to end of line     |
| `dd`     | Delete current line       |
| `2dd`    | Delete two lines          |
| `2w`     | Move forward two words    |
| `d2w`    | Delete two words          |
| `0`      | Move to beginning of line |
| `u`      | Undo                      |
| `U`      | Restore current line      |
| `Ctrl-r` | Redo                      |

---

### Key Idea

Most Vim editing commands follow:

```text
operator [count] motion
```

Examples:

```vim
dw
d$
d2w
3dd
```

Once this pattern becomes familiar, many other Vim commands become easy to learn because they follow the same structure.

💡 Beginner Tip:

Try reading commands from left to right:

```text
d2w
```

means:

```text
delete 2 words
```

This mental model makes Vim commands much easier to understand and remember.

# 🔵 VimTutor — Lesson 3: Put, Replace, and Change

Lesson 3 introduces one of Vim's greatest strengths:

**making edits quickly without repeatedly entering and leaving Insert mode.**

In this lesson, you'll learn how to:

* Restore deleted text
* Replace individual characters
* Change words and phrases efficiently
* Combine change commands with motions

These commands significantly speed up everyday editing.

---

## 3.1 The Put Command

When text is deleted using commands such as:

```vim
dd
```

or

```vim
dw
```

Vim stores the deleted text in a register.

To put (paste) the text back:

```vim
p
```

---

### Example

Before:

```text
d) Can you learn too?
b) Violets are blue,
c) Intelligence is learned,
a) Roses are red,
```

Delete the first line:

```vim
dd
```

Result:

```text
b) Violets are blue,
c) Intelligence is learned,
a) Roses are red,
```

Move the cursor to:

```text
c) Intelligence is learned,
```

Then press:

```vim
p
```

Result:

```text
b) Violets are blue,
c) Intelligence is learned,
d) Can you learn too?
a) Roses are red,
```

The deleted line is inserted **below** the cursor.

---

### Useful Notes

```vim
p
```

Paste after the cursor.

```vim
P
```

Paste before the cursor.

---

## 3.2 Replace a Character

For small corrections, entering Insert mode is often unnecessary.

Replace a single character using:

```vim
r
```

After pressing `r`, type the replacement character.

---

### Example

Incorrect text:

```text
Whan this lime was tuoed in
```

Move the cursor onto:

```text
Whan
   ^
```

Type:

```vim
re
```

Result:

```text
When
```

The character under the cursor is immediately replaced.

---

### Additional Examples

Replace:

```text
cat
 ^
```

with:

```vim
ro
```

Result:

```text
cot
```

This is extremely useful for fixing typos.

---

## 3.3 The Change Command

The **change operator** is one of Vim's most powerful editing commands.

It combines:

```text
delete + insert
```

into a single operation.

A common example:

```vim
ce
```

Meaning:

* `c` = change
* `e` = to end of word

After executing the command, Vim automatically enters Insert mode.

---

### Example

Incorrect word:

```text
lubw
```

Place the cursor on:

```text
l
```

Execute:

```vim
ce
```

Type:

```text
ine
```

Press:

```text
Esc
```

Result:

```text
line
```

The old text is removed and replaced immediately.

---

## 3.4 More Change Commands

Like delete commands, change commands follow a pattern:

```text
change [count] motion
```

Examples:

```vim
ce
cw
c$
c2w
```

---

### Change to End of Word

```vim
ce
```

Changes from the cursor position to the end of the current word.

---

### Change Word

```vim
cw
```

Changes the current word and immediately enters Insert mode.

Example:

Before:

```text
The quick fox
```

Cursor on:

```text
quick
```

Execute:

```vim
cw
```

Type:

```text
slow
```

Result:

```text
The slow fox
```

---

### Change to End of Line

```vim
c$
```

Deletes everything from the cursor position to the end of the line and enters Insert mode.

Example:

Before:

```text
I like apples and bananas
```

Cursor on:

```text
apples
```

Execute:

```vim
c$
```

Type:

```text
oranges
```

Result:

```text
I like oranges
```

---

### Change Multiple Words

```vim
c2w
```

Changes two words at once.

Example:

Before:

```text
The old broken laptop works
```

Cursor on:

```text
old
```

Execute:

```vim
c2w
```

Type:

```text
new
```

Result:

```text
The new laptop works
```

---

## 🎯 Lesson 3 Goal

By the end of Lesson 3, you should be able to:

* Restore deleted text
* Paste text using `p`
* Replace individual characters
* Use the `r` command for quick fixes
* Change words and phrases efficiently
* Understand how the change operator works

---

## 📌 Lesson 3 Summary

| Command   | Action                |
| --------- | --------------------- |
| `p`       | Paste after cursor    |
| `P`       | Paste before cursor   |
| `r<char>` | Replace one character |
| `ce`      | Change to end of word |
| `cw`      | Change current word   |
| `c$`      | Change to end of line |
| `c2w`     | Change two words      |
| `Esc`     | Return to Normal mode |

---

## Key Idea

```text
change = delete + insert
```

The change operator is often faster than:

```vim
dw
i
```

because it performs both actions in a single command.

Examples:

```vim
ce
cw
c$
c2w
```

Once you understand change commands, editing text in Vim becomes dramatically faster.

💡 Beginner Tip:

If you're unsure whether to use:

```vim
d
```

or

```vim
c
```

remember:

```text
d = delete
c = delete and immediately start typing replacement text
```

If you plan to replace text right away, `c` is usually the better choice.

# 🟣 VimTutor — Lesson 4: Search, Jump, and Substitute

Lesson 4 teaches how to move quickly through files, search for text, find matching brackets, and perform search-and-replace operations.

These are among the most frequently used Vim features when editing source code, configuration files, logs, and documentation.

Mastering these commands allows you to navigate large files efficiently and make changes quickly.

---

## 4.1 Cursor Location and File Status

Display information about the current file:

```vim
Ctrl-g
```

This shows:

* File name
* Current line number
* Total number of lines
* Cursor position within the file
* Percentage through the file

---

### Navigate Through a File

Go to the last line:

```vim
G
```

Go to the first line:

```vim
gg
```

Go directly to line 42:

```vim
42G
```

---

### Example

Suppose a file contains 500 lines.

```vim
500G
```

moves directly to line 500.

```vim
1G
```

or

```vim
gg
```

returns to the beginning of the file.

These commands are extremely useful when working with large files.

---

## 4.2 Search for Text

Search forward:

```vim
/word
```

Press:

```text
Enter
```

Vim jumps to the next occurrence of the word.

---

### Search Backward

Search in the opposite direction:

```vim
?word
```

Press:

```text
Enter
```

Vim searches upward through the file.

---

### Move Between Search Results

Next match in the same direction:

```vim
n
```

Next match in the opposite direction:

```vim
N
```

---

### Example

Search for:

```vim
/vim
```

Move to the next occurrence:

```vim
n
```

Move to the previous occurrence:

```vim
N
```

This makes navigating repeated text very fast.

---

## 4.3 Jump Between Matches

Vim remembers many cursor movements.

Move back to a previous location:

```vim
Ctrl-o
```

Move forward again:

```vim
Ctrl-i
```

Think of these as browser navigation buttons:

```text
Ctrl-o = Back
Ctrl-i = Forward
```

---

### Example Workflow

Search:

```vim
/config
```

Jump several matches:

```vim
n
n
n
```

Return to your previous position:

```vim
Ctrl-o
```

Move forward again:

```vim
Ctrl-i
```

This is extremely useful when exploring unfamiliar files.

---

## 4.4 Matching Parentheses and Brackets

Place the cursor on any matching character:

```text
( ) [ ] { }
```

Then press:

```vim
%
```

Vim jumps to the matching pair.

---

### Example

```text
if (count > 0) {
```

Cursor on:

```text
(
```

Press:

```vim
%
```

Result:

```text
)
```

The cursor jumps directly to the matching parenthesis.

---

### Another Example

```text
{
    if (x > 0) {
        printf("Hello");
    }
}
```

Place the cursor on:

```text
{
```

Press:

```vim
%
```

Vim jumps to the corresponding closing brace.

This is one of the most valuable commands when reading source code.

---

## 4.5 Substitute (Search and Replace)

Replace the first occurrence on the current line:

```vim
:s/old/new
```

Example:

```vim
:s/cat/dog
```

Only the first occurrence is replaced.

---

### Replace All Occurrences on the Current Line

```vim
:s/old/new/g
```

Example:

```vim
:s/cat/dog/g
```

The `g` means:

```text
global
```

which replaces all matches on that line.

---

### Example

Before:

```text
cat cat cat
```

Command:

```vim
:s/cat/dog/g
```

Result:

```text
dog dog dog
```

---

## 4.6 Replace in Multiple Lines

Replace text only within a specific range:

```vim
:10,20s/old/new/g
```

Meaning:

```text
Lines 10 through 20 only
```

Example:

```vim
:5,15s/error/success/g
```

Only lines 5 through 15 are affected.

Everything outside that range remains unchanged.

---

## 4.7 Replace in Entire File

Replace throughout the entire file:

```vim
:%s/old/new/g
```

The `%` symbol means:

```text
Entire file
```

---

### Example

```vim
:%s/error/success/g
```

Before:

```text
error found
error occurred
```

After:

```text
success found
success occurred
```

All matches are replaced.

---

## 4.8 Confirm Before Replacing

Sometimes you want to review each replacement.

Use:

```vim
:%s/old/new/gc
```

The `c` means:

```text
confirm
```

---

### Example Prompt

```text
replace with new? (y/n/a/q/l/^E/^Y)
```

Common options:

| Key | Meaning                        |
| --- | ------------------------------ |
| `y` | Yes                            |
| `n` | No                             |
| `a` | Replace all remaining matches  |
| `q` | Quit replacement               |
| `l` | Replace current match and stop |

This is useful when you need precise control over large changes.

---

## 🎯 Lesson 4 Goal

By the end of Lesson 4, you should be able to:

* Search forward and backward
* Navigate between matches
* Jump through location history
* Find matching brackets
* Perform search-and-replace operations
* Replace text across entire files
* Confirm replacements interactively

---

## 📌 Lesson 4 Summary

| Command          | Action                              |
| ---------------- | ----------------------------------- |
| `Ctrl-g`         | Show file status                    |
| `G`              | Go to end of file                   |
| `gg`             | Go to beginning of file             |
| `42G`            | Go to line 42                       |
| `/text`          | Search forward                      |
| `?text`          | Search backward                     |
| `n`              | Next match                          |
| `N`              | Previous/opposite match             |
| `Ctrl-o`         | Jump backward                       |
| `Ctrl-i`         | Jump forward                        |
| `%`              | Matching bracket                    |
| `:s/old/new`     | Replace first match on current line |
| `:s/old/new/g`   | Replace all matches on current line |
| `:%s/old/new/g`  | Replace all matches in file         |
| `:%s/old/new/gc` | Replace with confirmation           |

---

## Key Commands to Remember

```vim
/text
n
N
%
:%s/old/new/g
```

These commands provide fast navigation and powerful editing across entire files.

💡 Beginner Tip:

When editing a large file:

1. Search using:

```vim
/keyword
```

2. Move through matches using:

```vim
n
```

3. Use:

```vim
%
```

to navigate code blocks.

4. Use:

```vim
:%s/old/new/gc
```

for safe search-and-replace operations.

Together, these commands dramatically reduce the time needed to navigate and edit large files.

# 🔴 VimTutor — Lesson 5: External Commands and Files

Lesson 5 teaches how Vim interacts with the operating system, how to save text to files, and how to insert file contents or command output directly into your document.

These features allow Vim to work closely with your shell and filesystem, making it possible to perform many tasks without leaving the editor.

---

## 5.1 Run External Commands

Execute a shell command from inside Vim:

```vim
:!command
```

The `!` tells Vim to run an external command using your system shell.

---

### Examples

Show directory contents:

```vim
:!ls
```

Display the current working directory:

```vim
:!pwd
```

Show the current date and time:

```vim
:!date
```

Delete a file:

```vim
:!rm filename
```

⚠️ Be careful when running destructive commands such as:

```vim
:!rm filename
```

because they immediately affect files on your system.

---

### Example Workflow

Check the current directory:

```vim
:!pwd
```

View available files:

```vim
:!ls
```

Return to Vim and continue editing.

---

## 5.2 Save to a File

Write the current buffer to a file:

```vim
:w filename
```

Example:

```vim
:w notes.txt
```

This saves the current contents as:

```text
notes.txt
```

without leaving Vim.

---

### Save Under a Different Name

You can also use:

```vim
:w backup.txt
```

to create another copy of the current file.

This is similar to a "Save As" operation.

---

## 5.3 Visual Selection + Save

Vim allows you to save only a selected portion of text.

Start Visual mode:

```vim
v
```

Move the cursor to select text.

---

### Save the Selection

After selecting text, Vim automatically creates a range.

Write the selected text:

```vim
:'<,'>w section.txt
```

Example:

```vim
:'<,'>w notes.txt
```

This writes only the selected portion into:

```text
notes.txt
```

---

### Example

Before:

```text
Chapter 1
Chapter 2
Chapter 3
Chapter 4
```

Select:

```text
Chapter 2
Chapter 3
```

Then execute:

```vim
:'<,'>w section.txt
```

Result:

```text
section.txt
```

contains only:

```text
Chapter 2
Chapter 3
```

---

## 5.4 Read a File into the Current Document

Insert another file below the current cursor position:

```vim
:r filename
```

Example:

```vim
:r notes.txt
```

The contents of:

```text
notes.txt
```

are inserted below the current line.

---

### Example

Current document:

```text
Shopping List
```

Execute:

```vim
:r items.txt
```

If:

```text
items.txt
```

contains:

```text
Milk
Bread
Eggs
```

Result:

```text
Shopping List
Milk
Bread
Eggs
```

---

## 5.5 Read Command Output into a Document

Instead of reading a file, Vim can insert command output.

Syntax:

```vim
:r !command
```

The command runs in the shell and its output is inserted into the document.

---

### Example

Insert directory contents:

```vim
:r !ls
```

Possible result:

```text
file1.txt
file2.txt
notes.txt
```

---

### Insert Current Date

Execute:

```vim
:r !date
```

Possible result:

```text
Mon Jun 1 12:30:00 UTC 2026
```

This is useful for:

* Reports
* Logs
* Documentation
* Automatic timestamps
* Generated content

---

## Common Workflow Example

View available files:

```vim
:!ls
```

Save your work:

```vim
:w notes.txt
```

Insert the contents of a file:

```vim
:r notes.txt
```

Insert command output:

```vim
:r !date
```

Result:

```text
Mon Jun 1 12:30:00 UTC 2026
```

All of these operations can be performed without leaving Vim.

---

## 🎯 Lesson 5 Goal

By the end of Lesson 5, you should be able to:

* Run shell commands from Vim
* Save files using `:w`
* Save selected text into a separate file
* Insert another file into the current document
* Insert shell command output into a document
* Work with files without leaving Vim

---

## 📌 Lesson 5 Summary

| Command        | Action                     |
| -------------- | -------------------------- |
| `:!ls`         | List directory contents    |
| `:!pwd`        | Show current directory     |
| `:!date`       | Show current date and time |
| `:!rm file`    | Delete a file              |
| `:w file`      | Save as file               |
| `v`            | Start Visual mode          |
| `:'<,'>w file` | Save selected text         |
| `:r file`      | Insert file contents       |
| `:r !command`  | Insert command output      |

---

## Key Commands to Remember

```vim
:!command
:w filename
:r filename
:r !command
```

These commands connect Vim with the operating system, allowing you to interact with files and shell commands without leaving the editor.

💡 Beginner Tip:

When learning Vim, experiment with safe commands first:

```vim
:!pwd
:!ls
:r !date
```

Avoid commands that modify or delete files until you are comfortable working with the shell.

Once you understand these commands, Vim becomes much more than a text editor—it becomes a powerful interface to your entire system.

# 🟠 VimTutor — Lesson 6: Open, Copy, Replace, and Settings

Lesson 6 focuses on faster text editing techniques: opening new lines, appending text, copying (yanking), pasting, using Replace mode, and configuring Vim's search behavior.

These commands are used constantly in real-world editing and can significantly improve your speed and efficiency.

---

## 6.1 Open a New Line

Open a new line **below** the current line:

```vim
o
```

Vim immediately enters Insert mode.

---

### Example

Before:

```text
Line one
Line two
```

Cursor on:

```text
Line one
```

Press:

```vim
o
```

Result:

```text
Line one

Line two
```

You can immediately start typing on the new line.

---

### Open Above the Current Line

Open a new line **above** the current line:

```vim
O
```

This also enters Insert mode automatically.

---

### Example

Before:

```text
Line one
Line two
```

Cursor on:

```text
Line two
```

Press:

```vim
O
```

Result:

```text
Line one

Line two
```

A new blank line is inserted above the cursor.

---

## 6.2 Append Text

Insert text **after** the cursor:

```vim
a
```

Insert text at the **end of the current line**:

```vim
A
```

Both commands enter Insert mode.

---

### Difference Between `i` and `a`

Insert before the cursor:

```vim
i
```

Insert after the cursor:

```vim
a
```

---

### Example

Text:

```text
helo
   ^
```

Press:

```vim
a
```

Type:

```text
l
```

Result:

```text
hello
```

---

### Example Using `A`

Before:

```text
Hello
```

Press:

```vim
A
```

Type:

```text
 World
```

Result:

```text
Hello World
```

---

## 6.3 Move to End of Word

Move the cursor to the end of the current word:

```vim
e
```

---

### Example

Text:

```text
programming language
```

Starting at:

```text
programming language
^
```

Press:

```vim
e
```

Cursor moves to:

```text
programming language
          ^
```

Press:

```vim
e
```

again and Vim jumps to the end of the next word.

---

### Useful Navigation Commands

| Command | Action                |
| ------- | --------------------- |
| `w`     | Move to next word     |
| `b`     | Move to previous word |
| `e`     | Move to end of word   |

These motions are commonly combined with operators such as:

```vim
d
c
y
```

---

## 6.4 Yank (Copy) Text

In Vim, copying is called **yanking**.

Copy a word:

```vim
yw
```

Copy to the end of the line:

```vim
y$
```

Copy an entire line:

```vim
yy
```

---

### Examples

Copy one word:

```vim
yw
```

Copy two lines:

```vim
2yy
```

Copy three lines:

```vim
3yy
```

Like delete commands, yank commands support counts.

---

## 6.5 Paste Copied Text

Paste text after the cursor:

```vim
p
```

Paste text before the cursor:

```vim
P
```

---

### Duplicate a Line

Copy:

```vim
yy
```

Paste:

```vim
p
```

Result:

Before:

```text
Hello
```

After:

```text
Hello
Hello
```

This is one of the most frequently used Vim workflows.

---

### Example Using `P`

If a line is copied:

```vim
yy
```

Then:

```vim
P
```

pastes it above the current line instead of below.

---

## 6.6 Replace Mode

Enter Replace mode:

```vim
R
```

Unlike Insert mode, Replace mode overwrites existing text.

---

### Example

Before:

```text
hello world
```

Cursor on:

```text
hello world
      ^
```

Press:

```vim
R
```

Type:

```text
Vim
```

Result:

```text
hello Vimld
```

The existing characters are replaced as you type.

---

### Exit Replace Mode

Press:

```text
Esc
```

to return to Normal mode.

---

### Difference Between `r` and `R`

Replace one character:

```vim
r
```

Replace continuously:

```vim
R
```

Example:

```vim
rx
```

replaces only a single character.

---

## 6.7 Vim Settings

Vim allows many behaviors to be configured while editing.

---

### Ignore Case During Search

Enable case-insensitive searching:

```vim
:set ic
```

or:

```vim
:set ignorecase
```

Now:

```vim
/hello
```

matches:

```text
hello
HELLO
Hello
```

---

### Incremental Search

Show matches while typing:

```vim
:set is
```

or:

```vim
:set incsearch
```

Vim updates search results immediately as you type.

---

### Highlight Search Results

Highlight all matches:

```vim
:set hls
```

or:

```vim
:set hlsearch
```

This makes search results easier to spot.

---

### Disable an Option

Turn off ignorecase:

```vim
:set noic
```

General pattern:

```vim
:set no<option>
```

Examples:

```vim
:set nohlsearch
:set noignorecase
:set noincsearch
```

---

### Temporary vs Permanent Settings

Commands such as:

```vim
:set ic
```

only affect the current Vim session.

To make settings permanent, add them to your:

```text
~/.vimrc
```

file.

---

## 🎯 Lesson 6 Goal

By the end of Lesson 6, you should be able to:

* Open new lines above and below
* Append text efficiently
* Navigate by words
* Copy and paste text
* Duplicate lines quickly
* Use Replace mode
* Configure common Vim search settings

---

## 📌 Lesson 6 Summary

| Command     | Action                |
| ----------- | --------------------- |
| `o`         | Open line below       |
| `O`         | Open line above       |
| `a`         | Append after cursor   |
| `A`         | Append at end of line |
| `e`         | Move to end of word   |
| `yw`        | Copy word             |
| `yy`        | Copy line             |
| `2yy`       | Copy two lines        |
| `y$`        | Copy to end of line   |
| `p`         | Paste after cursor    |
| `P`         | Paste before cursor   |
| `R`         | Replace mode          |
| `:set ic`   | Ignore case           |
| `:set is`   | Incremental search    |
| `:set hls`  | Highlight matches     |
| `:set noic` | Disable ignore case   |

---

## Key Commands to Remember

```vim
o
O
yy
p
P
R
```

These commands dramatically speed up editing because they reduce the need to repeatedly enter and leave Insert mode.

💡 Beginner Tip:

A very common workflow is:

```vim
yy
p
```

This instantly duplicates the current line and is used constantly when editing configuration files, scripts, and source code.

Once you become comfortable with `o`, `yy`, `p`, and `R`, everyday editing becomes much faster and more efficient.

---

# ⚫ VimTutor — Lesson 7: Help, Configuration, and Completion

Lesson 7 is the final lesson of VimTutor.

It introduces Vim's built-in help system, window navigation, configuration files, and command-line completion.

These features are essential for continuing your Vim journey because Vim is designed to be:

* Self-documenting
* Highly configurable
* Extensible
* Efficient for long-term use

Once you complete this lesson, you'll have the foundation needed for everyday Vim usage and independent learning.

---

## 7.1 Open the Help System

Vim contains a complete built-in documentation system.

Open help:

```vim
:help
```

Or press:

```text
F1
```

A help window opens containing Vim documentation.

---

### Why Vim Help Is Important

Unlike many editors, almost every Vim feature is documented directly inside Vim.

When you forget a command, need clarification, or want to learn something new, the help system should be your first stop.

---

## 7.2 Get Help for a Specific Command

Look up information about a command:

```vim
:help dd
```

Help for searching:

```vim
:help search
```

Help for motion commands:

```vim
:help motion
```

Help for operators:

```vim
:help operator
```

Help for yank:

```vim
:help yank
```

Help for Visual mode:

```vim
:help visual-mode
```

---

### Examples

Want to learn about deleting?

```vim
:help dd
```

Want to learn about copy and paste?

```vim
:help yank
```

Want to learn about search?

```vim
:help search
```

The help system often provides examples and links to related topics.

---

## 7.3 Move Between Windows

When help opens, Vim usually splits the screen into multiple windows.

Switch between windows:

```vim
Ctrl-w Ctrl-w
```

Press repeatedly to cycle through open windows.

---

### Example

Current layout:

```text
+--------------------+
| Help Window        |
+--------------------+
| File Being Edited  |
+--------------------+
```

Switch focus:

```vim
Ctrl-w Ctrl-w
```

The cursor moves to the other window.

---

### Useful Window Navigation

Move to next window:

```vim
Ctrl-w Ctrl-w
```

Close current window:

```vim
:q
```

These commands are enough for basic help navigation.

---

## 7.4 Close the Help Window

Close the current help window:

```vim
:q
```

This closes only the active window.

If the help window is active:

```vim
:q
```

closes help while keeping your file open.

---

### Example Workflow

Open help:

```vim
:help
```

Read documentation.

Return to help window:

```vim
Ctrl-w Ctrl-w
```

Close help:

```vim
:q
```

Continue editing your file.

---

## 7.5 Create a Vim Configuration File

Vim automatically loads settings from:

```text
~/.vimrc
```

every time it starts.

This file allows you to customize Vim permanently.

---

### Example Configuration

```vim
set number
set ignorecase
set hlsearch
set incsearch
```

Meaning:

| Setting          | Purpose                            |
| ---------------- | ---------------------------------- |
| `set number`     | Show line numbers                  |
| `set ignorecase` | Ignore letter case while searching |
| `set hlsearch`   | Highlight search matches           |
| `set incsearch`  | Show matches while typing          |

---

### Example `.vimrc`

```vim
" Show line numbers
set number

" Better searching
set ignorecase
set hlsearch
set incsearch
```

Save the file and restart Vim.

The settings load automatically.

---

### Neovim Users

If you're using Neovim, configuration is commonly stored in:

```text
~/.config/nvim/init.lua
```

or:

```text
~/.config/nvim/init.vim
```

depending on your setup.

---

## 7.6 Command Completion

Vim can automatically complete commands, filenames, and help topics.

Start typing a command:

```vim
:e
```

Press:

```text
Ctrl-d
```

Vim displays possible completions.

---

### Autocomplete with Tab

Example:

```vim
:help se
```

Press:

```text
Tab
```

Result:

```vim
:help search
```

or another matching topic.

---

### Completion Works For

* Commands
* File names
* Directories
* Help topics
* Paths
* Vim options

---

### Examples

Edit a file:

```vim
:e no<Tab>
```

Possible completion:

```vim
:e notes.txt
```

Help topic:

```vim
:help vi<Tab>
```

Possible completion:

```vim
:help visual-mode
```

This saves time and reduces typing errors.

---

## Example Workflow

Open help:

```vim
:help
```

Search help for yank:

```vim
:help yank
```

Switch windows:

```vim
Ctrl-w Ctrl-w
```

Close help:

```vim
:q
```

Use autocomplete:

```vim
:he<Tab>
```

Result:

```vim
:help
```

This workflow demonstrates how Vim's documentation system can help you learn new commands directly inside the editor.

---

## 🎯 Lesson 7 Goal

By the end of Lesson 7, you should be able to:

* Open Vim help
* Search help topics
* Navigate between windows
* Close help windows
* Create a basic `.vimrc`
* Use command completion
* Continue learning Vim independently

---

# 📌 Lesson 7 Summary

| Command         | Action                    |
| --------------- | ------------------------- |
| `:help`         | Open help                 |
| `F1`            | Open help                 |
| `:help topic`   | Help for topic            |
| `Ctrl-w Ctrl-w` | Switch windows            |
| `:q`            | Close current window      |
| `~/.vimrc`      | Vim configuration file    |
| `Ctrl-d`        | Show possible completions |
| `Tab`           | Autocomplete              |
| `set number`    | Show line numbers         |
| `set hlsearch`  | Highlight matches         |

---

## Key Commands to Remember

```vim
:help
:help command
Ctrl-w Ctrl-w
```

These commands allow Vim to teach you Vim.

Whenever you encounter an unfamiliar command, remember:

```vim
:help <command>
```

For example:

```vim
:help dd
:help yank
:help search
```

The built-in help system is one of Vim's most powerful features and a major reason why experienced users can continue learning for years without leaving the editor.

---

## 🎉 Congratulations!

You have completed all seven VimTutor lessons.

You now understand:

* Navigation
* Editing
* Operators and motions
* Search and replace
* File operations
* Copy and paste
* Configuration
* Help and documentation

This foundation is enough to begin using Vim productively for everyday editing.

### Recommended Next Topics

After VimTutor, consider learning:

* Visual Mode
* Registers
* Marks
* Macros
* Buffers
* Window Management
* Tabs
* Text Objects
* Vim Plugins
* Neovim

Vim mastery comes from practice. Keep editing, keep exploring `:help`, and gradually build your workflow one command at a time.

---

## 📎 Official Documentation, Attribution, Copyright & Disclaimer

### 📖 Official Vim Documentation & References

This document is based on publicly available official sources and standard system behavior. The primary references include:

* Vim Official Website: [https://www.vim.org](https://www.vim.org)
* Built-in Vim Help System: run `:help` inside Vim (primary authoritative source)
* Vim User Manual (official documentation): [https://vimhelp.org](https://vimhelp.org)
* Vim Reference Manual (complete command specification): [https://vimhelp.org](https://vimhelp.org)
* Ubuntu Official Documentation: [https://help.ubuntu.com](https://help.ubuntu.com)
* GNU/Linux system behavior (POSIX-compliant shell tools and utilities)

These sources represent the **authoritative documentation layer** for all Vim-related commands, behaviors, and configurations mentioned in this guide.

---

### 📘 Nature of This Document

This file is:

* An **independent, unofficial learning summary**
* A **personal educational compilation of VimTutor lessons (1–7)**
* A structured reference created for **learning and revision purposes only**
* Not an official manual, documentation, or release from any software project

It is intended to help users understand Vim more easily by organizing concepts from official tutorials into simplified notes.

---

### ⚖️ Copyright & Ownership Notice

* Vim is free and open-source software originally created by **Bram Moolenaar** and maintained by contributors.
* Vim is distributed under the **Vim License**, a unique open-source license that encourages voluntary donations to charitable causes.
* Ubuntu is a registered trademark of **Canonical Ltd.**
* Linux is a registered trademark of **Linus Torvalds** and associated contributors.

All trademarks, names, and software references belong to their respective owners.

This document:

* Does **NOT claim ownership** of Vim, Ubuntu, Linux, or any related tools
* Does **NOT redistribute official Vim documentation**
* Does **NOT modify or replace official license terms**
* Uses references strictly for **educational and informational purposes**

---

### 🚫 Non-Affiliation & No Endorsement Statement

This project is **not affiliated with, sponsored by, endorsed by, or officially connected to**:

* Vim project or its maintainers
* Ubuntu / Canonical Ltd.
* Linux Foundation or Linux kernel contributors
* Any organization mentioned in this document

All references are purely for **educational explanation and learning clarity**.

---

### 📌 Usage Terms & Responsibility Disclaimer

By using this document, you acknowledge:

* This content is provided **as-is without any warranty**
* Accuracy is intended but not guaranteed
* Users should always verify commands using:

  * `:help` inside Vim (authoritative source)
  * Official Ubuntu/Linux documentation for system operations
* The author of this document is not responsible for:

  * Misuse of commands
  * System damage
  * Data loss
  * Incorrect interpretation of instructions

---

### 🧠 Best Practice Recommendations

For safe and correct usage:

* Always confirm Vim commands using `:help <command>`
* Prefer official documentation over third-party summaries for production use
* Test commands in a safe environment before applying them to critical systems
* Use Ubuntu or Linux manuals for OS-level operations
* Keep Vim updated via official package managers

---

### 📜 Licensing Understanding (Informational Only)

Vim uses a **Vim License (charityware-style license)** which:

* Allows free use, modification, and distribution
* Encourages optional donations to charitable causes (not mandatory)
* Requires preservation of license text and attribution in distributions

Careware/charityware models are common in open-source ecosystems where software is free but supported by voluntary contributions.

---

### 🧾 Final Statement

This document exists solely as a **learning aid and structured study guide**.
For authoritative definitions, behavior, or legal interpretation, always refer to:

* Official Vim documentation (`:help`)
* Vim.org
* Ubuntu documentation
* Respective upstream project maintainers