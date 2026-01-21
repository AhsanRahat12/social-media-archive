# Linux find Command

**Platform:** LinkedIn  
**Posted:** January 20, 2026  
**Topic:** Linux, Commands, DevOps

---

Spent 20 minutes searching for a file in Linux? The find command saves that time every single day.

Here's how to use it effectively.

**Basic syntax:**

find [directory] [criteria] [action]

**Most useful patterns I use daily:**

✅ Find files by name

find . -name "*.txt"
find /etc -name "docker*"

✅ Find by size (cleanup large files fast)

find /var/log -size +100M
find . -type f -empty

✅ Find and execute actions

find . -name "*.log" -mtime +30 -delete
find ~/scripts -name "*.sh" -exec chmod +x {} \;

✅ Limit search depth (performance boost)

find . -maxdepth 2 -name "*.py"

**One critical gotcha:**

Always quote your wildcards, or the shell expands them before find sees them.

❌ WRONG: find . -name *.txt
✅ RIGHT: find . -name "*.txt"

The find command turns manual file hunting into automated precision. Game changer for DevOps work.

What's your go-to find command pattern? Drop it below, I'd love to learn new tricks 👇

#Linux #DevOps #CommandLine #SysAdmin #CloudNative
