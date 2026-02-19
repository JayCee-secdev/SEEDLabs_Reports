# **Lab Report: Environment Variable & Set-UID Program**
## 🎯 Objective
To explore how environment variables affect the behavior of programs and investigate how they can be exploited to compromise the security of Set-UID (root-owned) programs.

## 🛠️ Environment & Tools
- System: Ubuntu (SEED VM) 
- Key Tools: gcc, chown, chmod, export, diff, strace
- Concepts: Process Forking, execve() vs system(), Capability Leaking.


## 💡 Key Learning & Tips
- The fork() Inheritance: When a parent process creates a child process using fork(), the child inherits an identical copy of the parent's environment variables.
- execve() vs. system(): * execve() is more secure because it requires environment variables to be explicitly passed and does not invoke a shell, preventing metacharacter exploitation.
- system() is dangerous in Set-UID programs because it invokes /bin/sh, which can be manipulated by environment variables like PATH.
- The "LD_LIBRARY_PATH" Safeguard: I observed that Ubuntu automatically strips LD_LIBRARY_PATH when a Set-UID program is executed. This is a built-in security measure to prevent users from forcing a privileged program to load malicious dynamic libraries (DLLs).
- PATH Manipulation: If a Set-UID program calls a command (like ls) using a relative path instead of an absolute path (like /bin/ls), an attacker can modify their PATH variable to point to a malicious executable with the same name.

## Troubleshooting Log
| Issue Encountered | Root Cause | Solution/Observation |
| --- | --- | --- |
| execve() not displaying variables | The envp[] array was set to NULL. | Explicitly pass the environ pointer to the execve() function. |
| Malicious ls not triggering root | dash shell in Ubuntu drops privileges by default. | Link /bin/sh to /bin/zsh (which doesn't drop privileges) to allow the exploit to work. |
| Capability Leak | A file descriptor (fd) was opened by root but not closed before dropping privileges. | Ensure all privileged file descriptors are closed before the program executes a shell or changes user IDs. |

## 🏁 Summary of Defense
The most critical takeaway from this lab is that Set-UID programs must never trust the user's environment.
1. Always use absolute paths for system calls.
2. Prefer execve() over system() to avoid shell-related vulnerabilities.
3. Properly manage file descriptors to prevent Capability Leaking, where a non-privileged shell inherits access to a root-opened file.
