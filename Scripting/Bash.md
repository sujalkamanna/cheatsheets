# Linux/Bash Scripting Cheatsheet

![Bash Logo](https://www.gnu.org/software/bash/bash.png)

---

## Table of Contents
1. [What is Bash Scripting?](#1-what-is-bash-scripting)
2. [Script Basics](#2-script-basics)
3. [Variables & Data Types](#3-variables--data-types)
4. [Operators](#4-operators)
5. [Control Structures](#5-control-structures)
6. [Functions](#6-functions)
7. [File Operations](#7-file-operations)
8. [String Manipulation](#8-string-manipulation)
9. [Arrays](#9-arrays)
10. [Advanced Techniques](#10-advanced-techniques)
11. [Best Practices](#11-best-practices)
12. [Practical Examples](#12-practical-examples)

---

## 1. What is Bash Scripting?

Bash (Bourne Again Shell) scripting is writing automated sequences of commands to perform specific tasks. Scripts automate repetitive tasks, system administration, backups, and software deployments without manual intervention.

**Key Benefits:**
- Automation of repetitive tasks
- System administration
- Task scheduling (cron)
- Batch processing
- Cross-platform compatibility
- No compilation needed
- Easy to learn and maintain
- Powerful command integration

---

## 2. Script Basics

### Creating a Script

```bash
# Method 1: Using text editor
nano script.sh

# Method 2: Using heredoc
cat > script.sh << 'EOF'
#!/bin/bash
echo "Hello, World!"
EOF

# Make executable
chmod +x script.sh

# Run script
./script.sh                    # Current directory
bash script.sh                 # Explicit bash
sh script.sh                   # Shell
source script.sh               # Run in current shell
```

### Shebang (Script Header)

```bash
#!/bin/bash                   # Bash shell
#!/bin/sh                     # POSIX shell (most compatible)
#!/usr/bin/env bash           # Find bash in PATH (recommended)
#!/usr/bin/env python3        # Python script
#!/usr/bin/env ruby           # Ruby script

# Check bash location
which bash
which sh
type bash
```

### Basic Script Structure

```bash
#!/bin/bash

# Comments start with #

# Script description
# Author: John Doe
# Date: 2024-01-01
# Description: This script does something

# Variables
VAR="value"

# Function definitions
function my_function() {
    echo "Doing something"
}

# Main execution
echo "Starting script"
my_function
echo "Script completed"

# Exit with status
exit 0                        # Success
exit 1                        # Error
```

### Script Execution Options

```bash
# Debug mode (show commands as executed)
bash -x script.sh
set -x                        # Enable in script
set +x                        # Disable in script

# Check syntax without executing
bash -n script.sh

# Combine options
bash -xn script.sh

# Strict mode (fail on errors)
set -e                        # Exit on error
set -u                        # Error on undefined variables
set -o pipefail               # Pipeline fails if any command fails
set -euo pipefail             # All strict settings

# At script start
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'                   # Set field separator
```

---

## 3. Variables & Data Types

### Variable Declaration & Usage

```bash
# Declare variable
VAR="value"
NAME="John Doe"
AGE=30
PATH="/usr/local/bin:$PATH"

# Use variable
echo $VAR                     # Print variable
echo ${VAR}                   # Explicit braces
echo "$VAR"                   # Quoted (preserves whitespace)
echo '$VAR'                   # Single quotes (literal)

# Variable naming rules
# - Start with letter or underscore
# - Contain letters, numbers, underscores
# - Case-sensitive
# - No spaces around =

# Best practices
MY_VAR="value"               # Uppercase for constants
my_var="value"               # Lowercase for variables
```

### Special Variables

```bash
# Command-line arguments
$0                            # Script name
$1, $2, $3, ...              # Arguments (1st, 2nd, 3rd)
$#                            # Number of arguments
$*                            # All arguments as single string
$@                            # All arguments as array (preferred)
$?                            # Exit status of last command
$$                            # Script's process ID (PID)
$!                            # PID of last background process

# Example script with arguments
#!/bin/bash
echo "Script: $0"
echo "First arg: $1"
echo "Second arg: $2"
echo "Total args: $#"
echo "All args: $@"
echo "Exit status of previous: $?"
```

### Variable Expansion & Manipulation

```bash
# Default values
${VAR:-default}              # Use default if VAR unset
${VAR:=default}              # Set and use default
${VAR:?Error message}        # Error if unset
${VAR:+alternate}            # Use alternate if set

# Substring extraction
${VAR:0:5}                   # First 5 characters
${VAR:3:2}                   # From position 3, length 2
${VAR:(-3)}                  # Last 3 characters

# String replacement
${VAR/old/new}               # Replace first occurrence
${VAR//old/new}              # Replace all occurrences
${VAR/#old/new}              # Replace at start
${VAR/%old/new}              # Replace at end

# String length
${#VAR}                      # Length of string
${#array[@]}                 # Array length

# Case conversion (bash 4+)
${VAR^^}                     # Convert to uppercase
${VAR,,}                     # Convert to lowercase
${VAR^}                      # Capitalize first letter
```

### Environment Variables

```bash
# Common environment variables
$HOME                        # User's home directory
$USER                        # Current username
$PWD                         # Current working directory
$PATH                        # Command search paths
$SHELL                       # Current shell
$TERM                        # Terminal type
$LANG                        # Language locale

# Set environment variable
export VAR="value"           # Available to child processes
VAR="value"                  # Local to current shell

# View environment variables
env                          # All environment variables
printenv                     # Display environment
echo $PATH

# Modify PATH
PATH="$PATH:/new/path"       # Add to PATH
export PATH                  # Make permanent in session
```

---

## 4. Operators

### Arithmetic Operators

```bash
# Arithmetic expansion
$((expression))              # Evaluate expression
a=$((5 + 3))                # a = 8
b=$((10 - 2))               # b = 8
c=$((4 * 5))                # c = 20
d=$((20 / 4))               # d = 5
e=$((17 % 5))               # e = 2 (modulo)
f=$((2 ** 3))               # f = 8 (exponent)

# Increment/decrement
((count++))                  # Post-increment
((++count))                  # Pre-increment
((count--))                  # Post-decrement
((--count))                  # Pre-decrement

# Assignment operators
((a += 5))                   # a = a + 5
((a -= 3))                   # a = a - 3
((a *= 2))                   # a = a * 2
((a /= 4))                   # a = a / 4
((a %= 3))                   # a = a % 3
```

### Comparison Operators

```bash
# Numeric comparison (use in [[ ]])
a -eq b                      # Equal
a -ne b                      # Not equal
a -lt b                      # Less than
a -le b                      # Less than or equal
a -gt b                      # Greater than
a -ge b                      # Greater than or equal

# String comparison
str1 = str2                  # Strings equal
str1 != str2                 # Strings not equal
-z str                       # String is empty
-n str                       # String is not empty
str1 < str2                  # str1 comes before str2 (lexically)
str1 > str2                  # str1 comes after str2

# File tests
-e file                      # File exists
-f file                      # Regular file
-d dir                       # Directory exists
-r file                      # File is readable
-w file                      # File is writable
-x file                      # File is executable
-s file                      # File size > 0
-L file                      # Symbolic link

# Logical operators
!                           # NOT
&&                          # AND (logical)
||                          # OR (logical)
```

### Test Commands

```bash
# test command (traditional)
test $a -gt 5
test -f file.txt
test -z "$string"

# [[ ]] (modern, recommended)
[[ $a -gt 5 ]]
[[ -f file.txt ]]
[[ -z "$string" ]]
[[ $a -gt 5 && $b -lt 10 ]]
[[ $a -gt 5 || $b -lt 10 ]]

# [ ] (POSIX compatible)
[ $a -gt 5 ]
[ -f file.txt ]
[ -z "$string" ]

# Arithmetic comparison (integers only)
(( a > 5 ))
(( a == b ))
(( a != b ))
```

---

## 5. Control Structures

### If Statements

```bash
# Simple if
if [ condition ]; then
    echo "Condition is true"
fi

# If-else
if [ condition ]; then
    echo "True"
else
    echo "False"
fi

# If-elif-else
if [ $age -lt 18 ]; then
    echo "Minor"
elif [ $age -lt 65 ]; then
    echo "Adult"
else
    echo "Senior"
fi

# Nested if
if [ condition1 ]; then
    if [ condition2 ]; then
        echo "Both true"
    fi
fi

# One-liner
if [ $a -eq 5 ]; then echo "Equal"; fi
[ $a -eq 5 ] && echo "Equal" || echo "Not equal"
```

### Loops

```bash
# For loop (range)
for i in {1..5}; do
    echo "Number: $i"
done

# For loop (list)
for fruit in apple banana orange; do
    echo "Fruit: $fruit"
done

# For loop (array)
arr=("one" "two" "three")
for item in "${arr[@]}"; do
    echo "Item: $item"
done

# For loop (C-style)
for ((i=1; i<=5; i++)); do
    echo "Count: $i"
done

# While loop
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    ((count++))
done

# Until loop (opposite of while)
count=1
until [ $count -gt 5 ]; do
    echo "Count: $count"
    ((count++))
done

# Loop control
break                        # Exit loop
continue                     # Skip to next iteration

# Infinite loop
while true; do
    echo "Running..."
    sleep 1
done
```

### Case Statement

```bash
# Case statement
case $var in
    pattern1)
        echo "Match 1"
        ;;
    pattern2|pattern3)
        echo "Match 2 or 3"
        ;;
    pattern*)
        echo "Pattern match"
        ;;
    *)
        echo "Default"
        ;;
esac

# Example
case $1 in
    start)
        echo "Starting service"
        ;;
    stop)
        echo "Stopping service"
        ;;
    restart)
        echo "Restarting service"
        ;;
    *)
        echo "Usage: $0 {start|stop|restart}"
        exit 1
        ;;
esac
```

---

## 6. Functions

### Function Definition

```bash
# Method 1: function keyword
function my_function() {
    echo "Hello from function"
    return 0
}

# Method 2: POSIX style (recommended)
my_function() {
    echo "Hello from function"
    return 0
}

# Call function
my_function

# Function with arguments
greet() {
    local name=$1
    local greeting=$2
    echo "$greeting, $name!"
}

greet "John" "Hello"

# Function with return value
add() {
    local a=$1
    local b=$2
    return $((a + b))
}

add 5 3
result=$?                    # Capture return value
echo "Result: $result"
```

### Function Variables & Scope

```bash
# Local variables (function scope)
my_function() {
    local var="local value"
    echo $var
}

# Global variable
global_var="global value"

my_function() {
    global_var="changed"     # Modifies global
}

# Function with local variable shadowing
var="outer"
my_function() {
    local var="inner"
    echo $var                # Prints "inner"
}

echo $var                    # Prints "outer"
```

### Advanced Functions

```bash
# Function with multiple parameters
process_file() {
    local input_file=$1
    local output_file=$2
    local format=${3:-json}  # Default parameter
    
    if [[ ! -f "$input_file" ]]; then
        echo "Error: File not found"
        return 1
    fi
    
    echo "Processing $input_file to $output_file as $format"
    return 0
}

# Usage
if process_file "input.txt" "output.txt" "csv"; then
    echo "Success"
else
    echo "Failed"
fi

# Function returning output
get_date() {
    echo "$(date +%Y-%m-%d)"
}

current_date=$(get_date)
echo "Today is: $current_date"
```

---

## 7. File Operations

### File Reading

```bash
# Read entire file
cat file.txt

# Read line by line
while IFS= read -r line; do
    echo "Line: $line"
done < file.txt

# Read into array
mapfile -t lines < file.txt
for line in "${lines[@]}"; do
    echo "$line"
done

# Read with field separator
while IFS=':' read -r field1 field2 field3; do
    echo "Field 1: $field1"
done < file.txt

# Read specific lines
head -n 10 file.txt         # First 10 lines
tail -n 10 file.txt         # Last 10 lines
sed -n '5,10p' file.txt     # Lines 5-10
```

### File Writing

```bash
# Write to file (overwrite)
echo "Hello" > file.txt

# Append to file
echo "World" >> file.txt

# Write multiple lines
cat > file.txt << EOF
Line 1
Line 2
Line 3
EOF

# Write without interpreting variables
cat > file.txt << 'EOF'
This will not expand $VAR
EOF

# Using tee (write and display)
echo "Content" | tee file.txt
echo "More" | tee -a file.txt

# Write from heredoc
cat << 'EOF' > script.sh
#!/bin/bash
echo "Hello"
EOF
```

### File Checking & Manipulation

```bash
# Check file/directory existence
if [ -f file.txt ]; then
    echo "File exists"
fi

if [ -d directory ]; then
    echo "Directory exists"
fi

# File properties
if [ -r file.txt ]; then echo "Readable"; fi
if [ -w file.txt ]; then echo "Writable"; fi
if [ -x file.txt ]; then echo "Executable"; fi
if [ -s file.txt ]; then echo "Has content"; fi

# File creation and deletion
touch new_file.txt          # Create empty file
rm file.txt                 # Delete file
mkdir new_dir               # Create directory
rmdir empty_dir             # Remove empty directory
rm -rf directory/           # Remove with contents

# File modification
chmod 755 file.txt          # Change permissions
chown user:group file.txt   # Change owner
cp source.txt dest.txt      # Copy file
mv old_name.txt new_name.txt  # Move/rename
```

---

## 8. String Manipulation

### Basic String Operations

```bash
# String concatenation
str1="Hello"
str2="World"
str3="$str1 $str2"
str4="${str1}${str2}"

# String length
length=${#str1}

# Substring
sub=${str1:0:3}             # First 3 characters (Hel)
sub=${str1:3:2}             # 2 chars from position 3 (lo)

# Find and replace
replaced="${str1//o/0}"      # Replace all o with 0
replaced="${str1/o/0}"       # Replace first o with 0
replaced="${str1/#H/J}"      # Replace at start
replaced="${str1/%d/k}"      # Replace at end
```

### String Testing

```bash
# String is empty
if [ -z "$str" ]; then
    echo "String is empty"
fi

# String is not empty
if [ -n "$str" ]; then
    echo "String has content"
fi

# String contains substring
if [[ "$str" == *"substring"* ]]; then
    echo "Contains substring"
fi

# String starts with
if [[ "$str" == prefix* ]]; then
    echo "Starts with prefix"
fi

# String ends with
if [[ "$str" == *suffix ]]; then
    echo "Ends with suffix"
fi

# Regular expression match
if [[ "$str" =~ ^[0-9]+$ ]]; then
    echo "Is number"
fi
```

### Case Conversion

```bash
# Uppercase (bash 4+)
upper="${str^^}"

# Lowercase (bash 4+)
lower="${str,,}"

# Using tr for older bash
upper=$(echo "$str" | tr '[:lower:]' '[:upper:]')
lower=$(echo "$str" | tr '[:upper:]' '[:lower:]')

# Capitalize first letter
capitalized="${str^}"

# Uncapitalize first letter
uncapitalized="${str,}"
```

---

## 9. Arrays

### Array Declaration & Access

```bash
# Indexed array
arr=("one" "two" "three")
arr[0]="one"
arr[1]="two"
arr[3]="four"               # Index 2 is skipped

# Associative array (key-value)
declare -A dict
dict[name]="John"
dict[age]="30"
dict[city]="New York"

# Access array element
echo ${arr[0]}              # First element
echo ${arr[@]}              # All elements
echo ${arr[*]}              # All as single string
echo ${#arr[@]}             # Array length
```

### Array Operations

```bash
# Add to array
arr+=("four")               # Append

# Loop through array
for item in "${arr[@]}"; do
    echo "$item"
done

# Loop with index
for i in "${!arr[@]}"; do
    echo "Index $i: ${arr[$i]}"
done

# Find element in array
if [[ " ${arr[@]} " =~ " two " ]]; then
    echo "Found"
fi

# Remove element
unset arr[1]                # Remove element at index 1

# Sort array
sorted=($(for item in "${arr[@]}"; do echo "$item"; done | sort))

# Join array with separator
IFS=','
joined="${arr[*]}"
unset IFS
```

### Associative Arrays

```bash
# Declare associative array
declare -A config
config[host]="localhost"
config[port]="8080"
config[user]="admin"

# Access element
echo ${config[host]}

# Loop through
for key in "${!config[@]}"; do
    echo "$key: ${config[$key]}"
done

# Check if key exists
if [[ -v config[port] ]]; then
    echo "Port is set"
fi
```

---

## 10. Advanced Techniques

### Command Substitution

```bash
# Using $() - preferred
result=$(command)
files=$(ls -la)
date_now=$(date +%Y-%m-%d)

# Using backticks (legacy)
result=`command`

# Nested substitution
nested=$(echo $(date))

# In strings
echo "Today is $(date +%A)"

# Process substitution
while read line; do
    echo "$line"
done < <(cat file.txt)
```

### Subshells

```bash
# Subshell in parentheses
(cd /tmp && pwd)            # Changes dir in subshell only
echo $PWD                   # Back to original directory

# Variables in subshell
var="outer"
(var="inner"; echo $var)    # Prints inner
echo $var                   # Prints outer

# Useful for parallel tasks
(long_task1) &
(long_task2) &
wait                        # Wait for all background jobs
```

### Input/Output Redirection

```bash
# Redirect output
command > file              # Overwrite
command >> file             # Append
command > /dev/null         # Discard output
command 2> error.log        # Redirect error
command 2>&1 file.log       # Both stdout and stderr
command &> file             # Both (shorthand)

# Redirect input
command < input.txt
while read line; do
    process "$line"
done < data.txt

# Here documents
cat << EOF
Multi-line
text with
variables: $VAR
EOF

# Here strings (bash)
grep "pattern" <<< "$text"
```

### Pipes and Filters

```bash
# Basic piping
command1 | command2

# Multiple pipes
cat file.txt | grep "pattern" | cut -d' ' -f1 | sort | uniq

# Pipeline with variables
data=$(echo "hello world" | tr '[:lower:]' '[:upper:]')

# Tee (duplicate output)
command | tee output.txt | grep "success"
```

### Error Handling

```bash
# Check exit status
command
if [ $? -ne 0 ]; then
    echo "Error occurred"
fi

# Short circuit evaluation
command && echo "Success" || echo "Failed"

# Trap errors
trap 'echo "Error on line $LINENO"' ERR
trap 'cleanup' EXIT

cleanup() {
    rm -f temp_file.txt
    echo "Cleanup done"
}

# Set error handling
set -e                      # Exit on error
set -u                      # Error on undefined variables
set -o pipefail             # Pipeline fails if any fails
set -euo pipefail           # All three

# Error messages to stderr
echo "Error message" >&2
```

---

## 11. Best Practices

### Code Style

```bash
# Use meaningful names
USER_NAME="John"            # Good
UN="John"                   # Bad

# Quote variables
echo "$VAR"                 # Good (handles spaces)
echo $VAR                   # Bad

# Use [[ ]] instead of [ ]
if [[ $name == "John" ]]; then  # Good
if [ $name = "John" ]; then     # Less ideal
fi

# Use local in functions
my_func() {
    local result            # Good
    result="value"
    echo "$result"
}

# Use meaningful comments
# Bad
i=0                         # Set i to 0

# Good
# Counter for iteration
i=0

# Use functions for reusability
# Bad
cp file.txt backup1.txt
cp file.txt backup2.txt

# Good
backup_file() {
    local file=$1
    cp "$file" "${file}.backup.$(date +%s)"
}
```

### Performance

```bash
# Use built-in commands when possible
# Avoid: result=$(cat file.txt | grep pattern)
# Better: grep pattern file.txt

# Use while read instead of for on large files
while IFS= read -r line; do
    process_line "$line"
done < huge_file.txt

# Avoid subshells when not needed
# Avoid: var=$(command)
# Better: command > file

# Use [[ ]] for pattern matching
# Faster than grep for simple matching
[[ $str =~ pattern ]] && echo "Match"

# Avoid unnecessary external commands
# Use bash parameter expansion
filename="${path##*/}"      # basename equivalent
dirname="${path%/*}"        # dirname equivalent
```

### Security

```bash
# Quote all variables
echo "$VAR"                 # Safe
echo $VAR                   # Unsafe - globbing and word splitting

# Use set -u to catch undefined variables
set -u
echo "$UNDEFINED_VAR"       # Errors

# Validate input
if [ $# -ne 2 ]; then
    echo "Usage: $0 arg1 arg2"
    exit 1
fi

# Escape user input
user_input="$1"
echo "${user_input//\"/\\\"}"  # Escape quotes

# Use readonly for constants
readonly CONFIG_FILE="/etc/myapp/config.conf"

# Don't use eval
# Bad: eval "$user_input"
# Better: use arrays or case statements
```

---

## 12. Practical Examples

### System Monitoring Script

```bash
#!/bin/bash

# System monitoring script

# Configuration
THRESHOLD_CPU=80
THRESHOLD_MEMORY=80
LOG_FILE="/var/log/system_monitor.log"

log_message() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" >> "$LOG_FILE"
}

check_cpu() {
    local cpu=$(top -bn1 | grep "Cpu(s)" | awk '{print int($2)}')
    
    if [ $cpu -gt $THRESHOLD_CPU ]; then
        log_message "WARNING: CPU usage is $cpu%"
        return 1
    fi
    return 0
}

check_memory() {
    local mem=$(free | grep Mem | awk '{printf("%.0f", $3/$2 * 100)}')
    
    if [ $mem -gt $THRESHOLD_MEMORY ]; then
        log_message "WARNING: Memory usage is $mem%"
        return 1
    fi
    return 0
}

check_disk() {
    local disk=$(df / | tail -1 | awk '{print $5}' | sed 's/%//')
    
    if [ $disk -gt 90 ]; then
        log_message "ERROR: Disk usage is $disk%"
        return 1
    fi
    return 0
}

main() {
    log_message "Starting system check"
    
    check_cpu
    check_memory
    check_disk
    
    log_message "System check completed"
}

main "$@"
```

### Backup Script

```bash
#!/bin/bash

# Backup script with rotation

BACKUP_DIR="/backups"
SOURCE_DIR="/home/user/documents"
RETENTION_DAYS=7
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="${BACKUP_DIR}/backup_${TIMESTAMP}.tar.gz"

# Create backup
backup() {
    echo "Creating backup..."
    
    if tar -czf "$BACKUP_FILE" -C "$(dirname "$SOURCE_DIR")" "$(basename "$SOURCE_DIR")"; then
        echo "Backup created: $BACKUP_FILE"
        return 0
    else
        echo "Error creating backup" >&2
        return 1
    fi
}

# Rotate old backups
rotate_backups() {
    echo "Removing old backups..."
    find "$BACKUP_DIR" -name "backup_*.tar.gz" -mtime +$RETENTION_DAYS -delete
}

# Verify backup
verify_backup() {
    echo "Verifying backup..."
    if tar -tzf "$BACKUP_FILE" > /dev/null; then
        echo "Backup verified successfully"
        return 0
    else
        echo "Backup verification failed" >&2
        return 1
    fi
}

main() {
    if [ ! -d "$BACKUP_DIR" ]; then
        mkdir -p "$BACKUP_DIR"
    fi
    
    backup && verify_backup && rotate_backups
}

main
```

### Deployment Script

```bash
#!/bin/bash

set -euo pipefail

# Application deployment script

readonly APP_NAME="myapp"
readonly APP_USER="appuser"
readonly APP_DIR="/opt/myapp"
readonly LOG_DIR="/var/log/myapp"

log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1"
}

error() {
    echo "[ERROR] $1" >&2
    exit 1
}

check_prerequisites() {
    log "Checking prerequisites..."
    
    [ -d "$APP_DIR" ] || error "App directory not found"
    command -v docker &> /dev/null || error "Docker not installed"
    [ -f "$APP_DIR/docker-compose.yml" ] || error "docker-compose.yml not found"
}

stop_application() {
    log "Stopping application..."
    cd "$APP_DIR"
    docker-compose down || error "Failed to stop application"
}

start_application() {
    log "Starting application..."
    cd "$APP_DIR"
    docker-compose up -d || error "Failed to start application"
}

health_check() {
    log "Performing health check..."
    
    local max_attempts=30
    local attempt=0
    
    while [ $attempt -lt $max_attempts ]; do
        if curl -f http://localhost:8080/health &> /dev/null; then
            log "Health check passed"
            return 0
        fi
        
        ((attempt++))
        sleep 2
    done
    
    error "Health check failed"
}

main() {
    log "Starting deployment for $APP_NAME"
    
    check_prerequisites
    stop_application
    start_application
    health_check
    
    log "Deployment completed successfully"
}

main "$@"
```

---

## Quick Reference

```bash
# Basics
#!/bin/bash
set -euo pipefail
VAR="value"
echo "Hello"

# Conditions
if [[ $x -eq 5 ]]; then
    echo "Equal"
fi

# Loops
for i in {1..5}; do echo $i; done
while [ $x -lt 10 ]; do ((x++)); done

# Functions
my_func() {
    local arg=$1
    echo "$arg"
    return 0
}

# Arrays
arr=("one" "two" "three")
echo "${arr[@]}"

# File operations
[ -f file.txt ] && echo "File exists"
[ -d dir ] && echo "Directory exists"

# String ops
${str:0:5}         # Substring
${str//old/new}    # Replace
${#str}            # Length

# Error handling
trap 'cleanup' EXIT
command || error "Failed"
```

---

## File Locations

| Item | Path |
|------|------|
| **Shell Config** | ~/.bashrc, ~/.bash_profile, /etc/bash.bashrc |
| **System Scripts** | /usr/local/bin, /usr/bin, /bin |
| **Cron Scripts** | /etc/cron.d, /etc/cron.daily |
| **Init Scripts** | /etc/init.d, /etc/systemd/system |
| **Log Files** | /var/log/ |

---

**Last Updated:** 2026
**Bash Version:** 5.x+
**License:** GPL v3

For detailed help: `man bash`, `bash --help`, `help command`

---

**Tips:**
- Always use `#!/bin/bash` shebang
- Test scripts with `bash -x script.sh`
- Quote all variables to prevent word splitting
- Use functions to make scripts modular
- Add error handling with `set -euo pipefail`
- Write meaningful comments
- Keep scripts readable and maintainable
- Test edge cases thoroughly