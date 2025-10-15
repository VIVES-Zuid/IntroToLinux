# 1. Introduction to Users and Groups


### Multi-User System Concepts
Linux is designed as a multi-user operating system where:
- Multiple users can use the system simultaneously
- Each user has their own identity, permissions, and environment
- Users can belong to multiple groups
- Groups allow sharing resources among multiple users
- Security is enforced through user and group permissions

### User Types:

#### System Users (UIDs 0-999)
- **root (UID 0)**: Superuser with unlimited privileges
- **System accounts (UIDs 1-999)**: Used by system services and daemons
- Examples: `www-data`, `mysql`, `sshd`, `nobody`

#### Regular Users (UIDs 1000+)
- Human users with limited privileges
- Can only modify files in their home directory
- Need `sudo` for administrative tasks
- Examples: `john`, `alice`, `student`

### Groups:
- **Primary group**: User's main group (usually same as username)
- **Secondary groups**: Additional groups for shared access
- **System groups**: Used by system services
- **User-defined groups**: Created for specific purposes
---

## Navigation

**Previous:** [← Learning Objectives](00-learning-objectives.md)  
**Next:** [→ User Information Files](02-user-information-files.md)  
**Lesson Home:** [↑ Lesson 09: Users Groups](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
