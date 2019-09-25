- Generate a secure random alpha-numeric string of 18 characters

```
LC_CTYPE=C;tr -dc '[:alnum:]' < /dev/urandom | head -c ${1:-18}; echo;
```
