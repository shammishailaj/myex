[Alpine Linux](../alpine-linux/) | [Docker](../docker/) | [S3CMD](../s3cmd/) | [Go](../go/) | [System](../system/) | [Security](../security/) | [MySQL](../mysql/) | [InfluxDB](../influxdb/)

- Generate a secure random alpha-numeric string of 18 characters

```
LC_CTYPE=C;tr -dc '[:alnum:]' < /dev/urandom | head -c ${1:-18}; echo;
```
