# 6. Practical Labs


### Lab 6.1: File Globbing Practice (15 minutes)

```bash
# 1. Create test files
mkdir globbing_test && cd globbing_test
touch file{1..10}.txt
touch {backup,config,temp}.log
touch test{a..e}.dat
touch image{01..05}.{jpg,png}

# 2. Practice basic globbing
ls *.txt
ls *.log
ls file?.txt
ls file[1-5].txt
ls *{1,5}*

# 3. Advanced patterns
ls *.{txt,log}
ls file[!1-3].txt
ls *{jpg,png}

# 4. Directory patterns
mkdir {src,docs,test}
touch src/*.c docs/*.md test/*.py
ls */*.c
ls **/*              # If globstar enabled

cd ..
```

### Lab 6.2: Archiving Practice (20 minutes)

```bash
# 1. Create test data
mkdir archive_test && cd archive_test
mkdir -p project/{src,docs,config}
echo "Source code" > project/src/main.c
echo "Documentation" > project/docs/README.md
echo "settings" > project/config/app.conf

# 2. Create archives
tar -cf project.tar project/
tar -czf project.tar.gz project/
tar -cjf project.tar.bz2 project/

# 3. Compare sizes
ls -lh project.tar*

# 4. Test extraction
mkdir restore_test
tar -xzf project.tar.gz -C restore_test/

# 5. Archive with exclusions
echo "temp" > project/temp.tmp
tar -czf clean_project.tar.gz --exclude='*.tmp' project/

cd ..
```

### Lab 6.3: Links Practice (15 minutes)

```bash
# 1. Create test file
mkdir links_test && cd links_test
echo "Original content" > original.txt

# 2. Create links
ln original.txt hardlink.txt
ln -s original.txt softlink.txt
ln -s ../links_test ../test_link

# 3. Examine links
ls -li
stat original.txt
file softlink.txt

# 4. Test link behavior
echo "Additional content" >> hardlink.txt
cat original.txt
cat softlink.txt

# 5. Test deletion
rm original.txt
cat hardlink.txt    # Still works
cat softlink.txt    # Broken link

cd ..
```

### Lab 6.4: dd and Block Operations (10 minutes)

```bash
# 1. Create test files
mkdir dd_test && cd dd_test

# 2. Create files of specific sizes
dd if=/dev/zero of=1MB.file bs=1M count=1
dd if=/dev/zero of=10MB.file bs=1M count=10

# 3. Check file sizes
ls -lh *.file

# 4. Copy with dd
dd if=1MB.file of=copy.file bs=4K

# 5. Create random data
dd if=/dev/urandom of=random.dat bs=1K count=5

# 6. Verify
ls -lh
file *.file *.dat

cd ..
```
---

## Navigation

**Previous:** [← Understanding Inodes](05-understanding-inodes.md)  
**Next:** [→ Best Practices](07-best-practices.md)  
**Lesson Home:** [↑ Lesson 06: Globbing Archiving Links](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
