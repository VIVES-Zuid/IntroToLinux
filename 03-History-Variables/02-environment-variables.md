# 2. Environment Variables


### What Are Environment Variables?
Environment variables are dynamic values that affect how processes run on your system. They store information about your system environment.

### Viewing Environment Variables:

```bash
# Show all environment variables
env

# Show all variables (including shell variables)
set

# Show specific variable
echo $VARIABLE_NAME
echo $HOME
echo $USER
echo $PATH
```

### Important Environment Variables:

#### $HOME
- Your home directory path
- Example: `/home/username`

```bash
echo $HOME
cd $HOME    # Same as: cd ~
```

#### $USER
- Your username

```bash
echo $USER
whoami      # Same information
```

#### $PWD
- Present Working Directory (current directory)

```bash
echo $PWD
pwd         # Same information
```

#### $PATH
- Directories where shell looks for commands
- Colon-separated list of directories

```bash
echo $PATH
# Example output: /usr/local/bin:/usr/bin:/bin:/usr/games
```

#### $PS1
- Primary shell prompt appearance

```bash
echo $PS1
# Example: \u@\h:\w\$
# \u = username, \h = hostname, \w = working directory
```

#### $SHELL
- Path to your current shell

```bash
echo $SHELL
# Example: /bin/bash
```

### Creating Variables:

```bash
# Create a variable (no spaces around =)
MY_VARIABLE="Hello World"
NAME="Linux Student"
NUMBER=42

# Use the variable
echo $MY_VARIABLE
echo "My name is $NAME"
echo "The answer is $NUMBER"
```

### Exporting Variables:
Variables created in shell are local by default. To make them available to child processes:

```bash
# Create and export in one step
export MY_GLOBAL_VAR="Available everywhere"

# Or create then export
MY_VAR="test"
export MY_VAR

# Check if variable is exported
env | grep MY_VAR
```
---

## Navigation

**Previous:** [← Review Shell Basics Lesson 12 Recap](01-review-shell-basics-lesson-12-recap.md)  
**Next:** [→ Command History](03-command-history.md)  
**Lesson Home:** [↑ Lesson 03: History Variables](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
