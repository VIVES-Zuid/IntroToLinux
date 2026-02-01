# 9. Troubleshooting Common Issues


### Command Not Found:
```bash
# Check if command exists
which command_name

# Check PATH
echo $PATH

# Check if file exists but not executable
ls -l /path/to/command
```

### Variable Not Set:
```bash
# Check if variable exists
echo $VAR_NAME

# List all variables
set | grep VAR_NAME

# Check if exported
env | grep VAR_NAME
```
---

## Navigation

**Next:** [→ Review Questions](10-review-questions.md)  
**Previous:** [← Advanced Environment Concepts](08-advanced-environment-concepts.md)  
**Lesson Home:** [↑ Lesson 3: History & Variables](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
