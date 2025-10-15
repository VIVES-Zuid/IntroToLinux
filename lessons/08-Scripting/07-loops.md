# 7. Loops


### for Loops:

#### Loop Over Lists:
```bash
#!/bin/bash

# Loop over a list of items
for fruit in apple orange banana grape; do
    echo "I like $fruit"
done

# Loop over files
for file in *.txt; do
    echo "Processing: $file"
    wc -l "$file"
done

# Loop over command output
for user in $(cut -d: -f1 /etc/passwd); do
    echo "User: $user"
done
```

#### Loop with Ranges:
```bash
#!/bin/bash

# Loop with number sequence
for i in {1..10}; do
    echo "Number: $i"
done

# Loop with step
for i in {0..20..2}; do  # 0 to 20, step 2
    echo "Even number: $i"
done

# C-style for loop
for ((i=1; i<=10; i++)); do
    echo "Counter: $i"
done
```

### while Loops:

```bash
#!/bin/bash

# Basic while loop
counter=1
while [ $counter -le 5 ]; do
    echo "Count: $counter"
    counter=$((counter + 1))
done

# Read file line by line
while read -r line; do
    echo "Line: $line"
done < /etc/passwd

# Infinite loop with break condition
while true; do
    read -p "Enter 'quit' to exit: " input
    if [ "$input" = "quit" ]; then
        break
    fi
    echo "You entered: $input"
done
```

### until Loops:

```bash
#!/bin/bash

# until loop (opposite of while)
counter=1
until [ $counter -gt 5 ]; do
    echo "Count: $counter"
    counter=$((counter + 1))
done

# Wait for file to exist
until [ -f "/tmp/ready.flag" ]; do
    echo "Waiting for ready signal..."
    sleep 1
done
echo "Ready signal received!"
```
---

## Navigation

**Previous:** [← Conditional Statements](06-conditional-statements.md)  
**Next:** [→ Case Statements](08-case-statements.md)  
**Lesson Home:** [↑ Lesson 08: Scripting](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
