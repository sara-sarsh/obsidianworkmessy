one of the major problems in the devices is that they have 



I'll help you find and analyze large files in the /opt/evck directory.

Looking at your current location in `/opt/evck/logs`, let me check the size of files here and in the parent directory:

```bash
# Check size of current directory and subdirectories
du -sh /opt/evck/*

# Find files larger than 10MB in /opt/evck
find /opt/evck -type f -size +10M -exec ls -lh {} \; 2>/dev/null

# Check disk usage of the logs directory specifically
du -sh /opt/evck/logs
ls -lh /opt/evck/logs/
```

Run these commands to see:
1. Size of each directory/file in /opt/evck
2. Any files larger than 10MB
3. Detailed size of each log file

The uncompressed logs (`.log` files without `.gz`) are likely the largest. You can safely remove old compressed logs or uncompressed logs from July/August if you don't need them.

To remove old logs, you could:
```bash
# Remove compressed logs older than 30 days
find /opt/evck/logs -name "*.log.gz" -mtime +30 -delete

# Or remove all July and August logs
rm /opt/evck/logs/evck-2026-07-*.log.gz
rm /opt/evck/logs/evck-2026-08-*.log.gz
```

What would you like me to help you remove?