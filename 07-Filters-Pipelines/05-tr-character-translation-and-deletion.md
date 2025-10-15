# 5. tr: Character Translation and Deletion


tr translates or deletes characters.

### Character Translation:

```bash
# Convert lowercase to uppercase
echo "hello world" | tr 'a-z' 'A-Z'
echo "hello world" | tr '[:lower:]' '[:upper:]'

# Convert uppercase to lowercase
echo "HELLO WORLD" | tr 'A-Z' 'a-z'

# Replace specific characters
echo "hello world" | tr ' ' '_'    # Replace spaces with underscores
echo "hello,world" | tr ',' ' '    # Replace commas with spaces

# Multiple character replacement
echo "hello123world" | tr '123' 'abc'  # Replace digits with letters
```

### Character Deletion:

```bash
# Delete specific characters
echo "hello world" | tr -d ' '     # Remove spaces
echo "abc123def" | tr -d '0-9'     # Remove digits
echo "hello" | tr -d 'l'           # Remove letter 'l'

# Delete character sets
echo "Hello123World!" | tr -d '[:digit:]'  # Remove digits
echo "Hello World!" | tr -d '[:punct:]'    # Remove punctuation
```

### Character Squeezing:

```bash
# Squeeze repeated characters
echo "hello    world" | tr -s ' '  # Squeeze multiple spaces to one
echo "aaaaabbbbcccc" | tr -s 'abc' # Squeeze repeated a, b, c

# Common uses
cat file.txt | tr -s '\n'          # Remove blank lines
echo "text" | tr -s '[:space:]'    # Normalize whitespace
```

### Practical tr Examples:

```bash
# Clean data
cat data.txt | tr -d '\r'          # Remove Windows line endings
cat data.txt | tr '\t' ','          # Convert tabs to commas

# Format text
echo "file name with spaces.txt" | tr ' ' '_'  # file_name_with_spaces.txt

# ROT13 encoding
echo "hello" | tr 'a-zA-Z' 'n-za-mN-ZA-M'

# Extract alphanumeric only
echo "Hello, World! 123" | tr -cd '[:alnum:]'
```
---

## Navigation

**Previous:** [← Cut Extract Fields And Characters](04-cut-extract-fields-and-characters.md)  
**Next:** [→ Wc Word Line And Character Counting](06-wc-word-line-and-character-counting.md)  
**Lesson Home:** [↑ Lesson 07: Filters Pipelines](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
