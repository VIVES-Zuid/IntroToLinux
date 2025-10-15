# 10. Practical Labs


### Lab 8.1: Basic Script Creation (15 minutes)

```bash
# 1. Create a simple greeting script
cat > greeting.sh << 'EOF'
#!/bin/bash
# Simple greeting script

echo "Hello! Welcome to shell scripting!"
echo "Today is: $(date)"
echo "You are logged in as: $USER"
echo "Your home directory is: $HOME"
EOF

# 2. Make executable and test
chmod +x greeting.sh
./greeting.sh

# 3. Create an interactive script
cat > interactive.sh << 'EOF'
#!/bin/bash
# Interactive script

read -p "What's your name? " name
read -p "What's your favorite color? " color

echo "Hello, $name!"
echo "Your favorite color is $color."
echo "Nice to meet you!"
EOF

chmod +x interactive.sh
./interactive.sh
```

### Lab 8.2: File Processing Script (20 minutes)

```bash
# 1. Create test files
mkdir script_test && cd script_test
touch file{1..5}.txt
touch document{1..3}.pdf
touch image{1..2}.jpg

# 2. Create file counter script
cat > count_files.sh << 'EOF'
#!/bin/bash
# Count files by extension

echo "File Statistics for: $(pwd)"
echo "===================="

for ext in txt pdf jpg; do
    count=$(ls *.$ext 2>/dev/null | wc -l)
    echo "$ext files: $count"
done

echo "===================="
echo "Total files: $(ls -1 | wc -l)"
EOF

chmod +x count_files.sh
./count_files.sh
```

### Lab 8.3: Conditional Logic Practice (20 minutes)

```bash
# Create file checker script
cat > file_checker.sh << 'EOF'
#!/bin/bash
# File and directory checker

if [ $# -eq 0 ]; then
    echo "Usage: $0 <file_or_directory>"
    exit 1
fi

TARGET="$1"

echo "Checking: $TARGET"
echo "=================="

if [ -e "$TARGET" ]; then
    echo "✓ Exists"
    
    if [ -f "$TARGET" ]; then
        echo "✓ Is a file"
        echo "  Size: $(stat -c%s "$TARGET") bytes"
        
        if [ -r "$TARGET" ]; then
            echo "  ✓ Readable"
        fi
        
        if [ -w "$TARGET" ]; then
            echo "  ✓ Writable"
        fi
        
        if [ -x "$TARGET" ]; then
            echo "  ✓ Executable"
        fi
        
    elif [ -d "$TARGET" ]; then
        echo "✓ Is a directory"
        file_count=$(ls -1 "$TARGET" 2>/dev/null | wc -l)
        echo "  Contains: $file_count items"
    fi
else
    echo "✗ Does not exist"
fi
EOF

chmod +x file_checker.sh

# Test the script
./file_checker.sh count_files.sh
./file_checker.sh .
./file_checker.sh nonexistent
```

### Lab 8.4: Loop Practice (15 minutes)

```bash
# Create system monitor script
cat > system_monitor.sh << 'EOF'
#!/bin/bash
# System monitoring with loops

echo "System Monitor"
echo "============="

# Monitor for 5 iterations
for i in {1..5}; do
    echo "Iteration $i/5"
    echo "Time: $(date)"
    echo "Load: $(uptime | awk -F'load average:' '{print $2}')"
    echo "Memory: $(free -m | grep '^Mem:' | awk '{printf "%.1f%%", $3/$2 * 100}')"
    echo "---"
    
    if [ $i -lt 5 ]; then
        sleep 2
    fi
done

echo "Monitoring complete!"
EOF

chmod +x system_monitor.sh
./system_monitor.sh
```
---

## Navigation

**Previous:** [← Script Examples And Best Practices](09-script-examples-and-best-practices.md)  
**Next:** [→ Review Questions](11-review-questions.md)  
**Lesson Home:** [↑ Lesson 08: Scripting](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
