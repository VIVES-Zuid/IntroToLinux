# 3. Command History


### History Features:
The shell remembers commands you've typed, making it easy to repeat or modify previous commands.

### History Commands:

```bash
# Show command history
history

# Show last 10 commands
history 10

# Clear history
history -c

# Search history
history | grep keyword
```

### History Navigation Shortcuts:

```bash
# Previous command
!!

# Previous command with modification
!!:s/old/new/

# Command from history by number
!number
!150

# Last command starting with specific text
!text
!ls    # Last command starting with 'ls'

# Previous command's last argument
!$
```

### Interactive History Search:
- **Ctrl+R**: Reverse search through history
- Type to search, Enter to execute, Ctrl+R again for next match
- Ctrl+C to cancel search

### Arrow Keys:
- **Up Arrow**: Previous command
- **Down Arrow**: Next command
- **Left/Right Arrows**: Move cursor in current command
---

## Navigation

**Previous:** [← Environment Variables](02-environment-variables.md)  
**Next:** [→ Command Line Editing](04-command-line-editing.md)  
**Lesson Home:** [↑ Lesson 03: History Variables](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
