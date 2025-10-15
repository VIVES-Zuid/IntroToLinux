# 11. Building Powerful Pipelines

## Pipeline Design Patterns

```
Basic Pipeline Pattern:
┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐
│ Input   │──▶│ Filter  │──▶│ Process │──▶│ Output  │
│ Source  │   │ Data    │   │ Data    │   │ Format  │
└─────────┘   └─────────┘   └─────────┘   └─────────┘

Multi-stage Processing:
┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐
│  Raw    │──▶│ Clean   │──▶│ Extract │──▶│  Sort   │──▶│ Format  │
│  Data   │   │  Data   │   │ Fields  │   │  Data   │   │ Output  │
└─────────┘   └─────────┘   └─────────┘   └─────────┘   └─────────┘

Parallel Processing:
              ┌─────────┐
         ┌───▶│Process A│────┐
┌─────────┐   └─────────┘    ▼   ┌─────────┐
│ Input   │                     │ Combine │──▶ Output
│ Source  │   ┌─────────┐    ▲   │ Results │
└─────────┘   │Process B│────┘   └─────────┘
         └───▶└─────────┘
```

### Example Pipeline Patterns:

```bash
# Data analysis pipeline
cat data.csv | \
    grep -v "^#" | \               # Remove comments
    cut -d, -f2,4 | \              # Extract columns 2 and 4
    grep -v "^$" | \               # Remove empty lines
    sort | \                       # Sort the data
    uniq -c | \                    # Count occurrences
    sort -nr | \                   # Sort by count (descending)
    head -10                       # Top 10 results

# Log analysis pipeline
grep "$(date '+%Y-%m-%d')" /var/log/syslog | \
    grep "ERROR" | \
    cut -d' ' -f5- | \
    sort | \
    uniq -c | \
    sort -nr

# System monitoring pipeline
ps aux | \
    grep -v "^USER" | \            # Remove header
    awk '{print $4, $11}' | \      # Memory % and command
    sort -nr | \                   # Sort by memory usage
    head -10 | \                   # Top 10 processes
    while read mem cmd; do \       # Format output
        printf "%.1f%% %s\n" "$mem" "$cmd"
    done
```

## Pipeline Visualization Example

```
Real-world Log Analysis Pipeline:

cat access.log | grep "404" | cut -d' ' -f1 | sort | uniq -c | sort -nr | head -5

Step 1: Read log        Step 2: Filter 404s    Step 3: Extract IPs
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│192.168.1.1 200  │    │192.168.1.1 404  │    │192.168.1.1      │
│192.168.1.2 404  │───▶│192.168.1.3 404  │───▶│192.168.1.3      │
│192.168.1.3 404  │    │192.168.1.1 404  │    │192.168.1.1      │
│192.168.1.1 404  │    │192.168.1.5 404  │    │192.168.1.5      │
│192.168.1.4 200  │    └─────────────────┘    └─────────────────┘
│192.168.1.5 404  │
└─────────────────┘

Step 4: Sort IPs        Step 5: Count unique   Step 6: Sort by count
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│192.168.1.1      │    │   2 192.168.1.1 │    │   2 192.168.1.1 │
│192.168.1.1      │───▶│   1 192.168.1.3 │───▶│   1 192.168.1.5 │
│192.168.1.3      │    │   1 192.168.1.5 │    │   1 192.168.1.3 │
│192.168.1.5      │    └─────────────────┘    └─────────────────┘
└─────────────────┘
```

### Text Processing Workflow:

```bash
# Clean and analyze text file
cat document.txt | \
    tr '[:upper:]' '[:lower:]' | \ # Convert to lowercase
    tr -d '[:punct:]' | \          # Remove punctuation
    tr ' ' '\n' | \                # One word per line
    grep -v "^$" | \               # Remove empty lines
    sort | \                       # Sort words
    uniq -c | \                    # Count word frequency
    sort -nr | \                   # Sort by frequency
    head -20 > word_frequency.txt  # Save top 20 words
```
---

## Navigation

**Previous:** [← Sed Stream Editor](10-sed-stream-editor.md)  
**Next:** [→ Practical Labs](12-practical-labs.md)  
**Lesson Home:** [↑ Lesson 07: Filters Pipelines](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
