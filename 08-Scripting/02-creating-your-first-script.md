# 2. Creating Your First Script

### Anatomy of a Script

```
 hello-world.sh
┌─────────────────────────────────────────┐
│ #!/bin/bash                             │ ◀── shebang: "run me with bash"  (must be line 1)
│ # Script: hello-world.sh                │ ◀── comments: ignored by bash, for humans
│ # Purpose: my first script              │
│                                         │
│ echo "Hello, World!"                    │ ◀── commands: executed top to bottom
│ echo "This is my first shell script"    │
└─────────────────────────────────────────┘
```

### The Shebang Line

- `#!/bin/bash` tells Linux **which program** must run the script
- It must be the **very first line** (no empty line or space before it)
- `#!` is pronounced "she-bang" (hash + bang)

### Three Steps: Create, Make Executable, Run

```mermaid
graph LR
    A["1️⃣ Create<br/>nano hello-world.sh"] --> B["2️⃣ Make executable<br/>chmod +x hello-world.sh"]
    B --> C["3️⃣ Run<br/>./hello-world.sh"]
```

```bash
# 1. Create the script file
nano hello-world.sh

# 2. Make it executable (x = execute permission, see lesson 10)
chmod +x hello-world.sh

# 3. Run it
./hello-world.sh
```

### Why `./` in Front?

When you type a command, bash only looks in the directories of your `$PATH` (like `/usr/bin`). Your current directory is **not** in that list. `./` means "the file in **this** directory".

```bash
hello-world.sh      # ❌ command not found
./hello-world.sh    # ✅ runs the script in the current directory
```

### Running Without chmod

You can also start bash yourself and give it the script. Then the execute permission is not needed:

```bash
bash hello-world.sh
```

### Common Errors

| Error | Cause | Fix |
|-------|-------|-----|
| `Permission denied` | no execute permission | `chmod +x script.sh` |
| `command not found` | forgot `./` | `./script.sh` |
| `bad interpreter` | typo in the shebang | check line 1: `#!/bin/bash` |

> ℹ️ **Advanced** – To run your script from anywhere without `./`, copy it to a directory in your `PATH`, for example `sudo cp script.sh /usr/local/bin/`. Another common shebang is `#!/usr/bin/env bash`, which searches for bash in the `PATH`.

---

## Navigation

**Next:** [→ Comments And Documentation](03-comments-and-documentation.md)  
**Previous:** [← Introduction To Shell Scripting](01-introduction-to-shell-scripting.md)  
**Lesson Home:** [↑ Lesson 8: Shell Scripting Fundamentals](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
