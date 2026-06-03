# Shell script interview Q&As for Java developers   Java Success.com

## Table of Contents

- [Q8: Can you write a sample code to parse command line ar guments passed to the scrip](#q8)

---

## Q8: Can you write a sample code to parse command line ar guments passed to the script as “-date 2014-12-10”, etc?

**Answer:**

for user in $(cut -f1 -d: /etc/passwd ); do echo $user; crontab -l $user; done
root:x:0:1:Super -User :/:/usr/bin/ksh
daemon :x:1:1::/:
bin:x:2:2::/usr/bin:
.....

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
