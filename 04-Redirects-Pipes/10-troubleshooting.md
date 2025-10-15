# 10. Troubleshooting


### Common Issues:

#### Broken Pipe Errors:
- Occur when reader terminates before writer
- Usually harmless in interactive use

#### Permission Denied:
```bash
# Can't write to file
echo "test" > /etc/hosts  # Permission denied

# Solution: use sudo
echo "test" | sudo tee -a /etc/hosts
```

#### No Output from Pipeline:
- Check each command separately
- Use intermediate files for debugging

```bash
# Debug pipeline step by step
command1 > temp1.txt
command2 < temp1.txt > temp2.txt
command3 < temp2.txt
```
---

## Navigation

**Previous:** [← Performance And Best Practices](09-performance-and-best-practices.md)  
**Next:** [→ Review Questions](11-review-questions.md)  
**Lesson Home:** [↑ Lesson 04: Redirects Pipes](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
