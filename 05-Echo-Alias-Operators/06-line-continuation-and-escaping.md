# 6. Line Continuation and Escaping


### Backslash (\): Line Continuation

```bash
# Break long commands across lines
command1 \
    --option1 value1 \
    --option2 value2 \
    --option3 value3

# Real example
find /home \
    -name "*.txt" \
    -type f \
    -exec grep -l "pattern" {} \;

# With pipes
cat file.txt | \
    grep "pattern" | \
    sort | \
    uniq -c | \
    sort -nr
```

### Escape Characters:

```bash
# Escape special characters
echo "The price is \$10"
echo "Path: C:\\Users\\Admin"
echo "Quote: \"Hello World\""

# Escape in file names
touch file\ with\ spaces.txt
ls file\ with\ spaces.txt
```
---

## Navigation

**Next:** [→ Practical Labs](07-practical-labs.md)  
**Previous:** [← Control Operators](05-control-operators.md)  
**Lesson Home:** [↑ Lesson 5: Echo, Alias & Operators](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
