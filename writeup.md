## I encourage everyone to try completing the tasks without any help. Only use this sheet if you're stuck.

## Table of content
- [Task 1  Introduction](#task-1---Introduction)
- [Task 2  Find all the flags](#task-2---Find-all-the-flags)

# Task 1 - Introduction:
Deploy the machine and connect to our network. 
```
No answer neeeded
```

# Task 2  - Find all the flags:

# Port Scan
It identified ports 21,22,80,111,2049 and 3306 as open. Running a more detailed scan on found open ports with `nmap` reveals their respective versions.
```
nmap -p- -sS -sV -sC -A -O -T5 <machine-ip> -v 
```
