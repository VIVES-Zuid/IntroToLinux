# 2. The cat Command and tee Utility

### cat: Display and Concatenate Files

`cat` = con**cat**enate: it glues files together and writes them to the screen.

```
 file1.txt ─┐
            ├──▶ cat ──▶ screen (or > combined.txt)
 file2.txt ─┘
```

```bash
# Display file contents
cat file.txt

# Display multiple files after each other
cat file1.txt file2.txt file3.txt

# Concatenate files into a new file
cat file1.txt file2.txt > combined.txt

# Number lines
cat -n file.txt

# Show hidden characters (tabs = ^I, line ends = $)
cat -A file.txt

# Squeeze multiple blank lines into one
cat -s file.txt

# Create a file with cat (type text, end with Ctrl+D)
cat > newfile.txt
This is content
More content
^D
```

### tee: Split the Output Stream

`tee` works like a **T-junction** in a water pipe: the data goes to a file **and** continues down the pipeline.

```
                     ┌──▶ output.txt   (copy saved)
 command ──▶ tee ────┤
                     └──▶ next command / screen
```

```bash
# Save to a file AND show on screen
ls -l | tee output.txt

# Save to multiple files
ls -l | tee file1.txt file2.txt

# Append instead of overwrite
date | tee -a logfile.txt

# Save an intermediate result, keep working with the data
ls -la | tee directory_listing.txt | wc -l
cat /etc/passwd | tee users_backup.txt | head -5
```

> 💡 `tee` is very handy to **debug** a pipeline: put `tee step1.txt` in the middle to see what the data looks like at that point.

---

## Navigation

**Next:** [→ Grep Pattern Matching Master](03-grep-pattern-matching-master.md)  
**Previous:** [← Understanding Filters](01-understanding-filters.md)  
**Lesson Home:** [↑ Lesson 7: Text Processing and Filters](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
