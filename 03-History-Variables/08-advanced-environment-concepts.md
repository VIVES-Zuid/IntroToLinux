# 8. Advanced Environment Concepts


### Variable Types:

#### Local Variables:
- Only available in current shell
- Not inherited by child processes

```bash
LOCAL_VAR="Only here"
```

#### Environment Variables:
- Available to current shell and child processes
- Created with `export`

```bash
export GLOBAL_VAR="Everywhere"
```

#### Special Variables:
- `$?` - Exit status of last command (0 = success)
- `$$` - Process ID of current shell
- `$!` - Process ID of last background command

```bash
echo "Last command status: $?"
echo "Shell process ID: $$"
```

### Variable Manipulation:

```bash
# String length
VAR="Hello World"
echo ${#VAR}        # Output: 11

# Substring
echo ${VAR:0:5}     # Output: Hello
echo ${VAR:6}       # Output: World

# Default values
echo ${VAR:-"default"}      # Use VAR or "default" if empty
echo ${VAR:="default"}      # Set VAR to "default" if empty
```
---

## Navigation

**Previous:** [← Practical Lab Environment Setup](07-practical-lab-environment-setup.md)  
**Next:** [→ Troubleshooting Common Issues](09-troubleshooting-common-issues.md)  
**Lesson Home:** [↑ Lesson 03: History Variables](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
