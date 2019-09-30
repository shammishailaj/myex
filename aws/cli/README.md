[Home](../) | [Alpine Linux](../alpine-linux/) | [Docker](../docker/) | [S3CMD](../s3cmd/) | [Go](../go/)
[System](../system/) | [Security](../security/) | [MySQL](../mysql/) | [InfluxDB](../influxdb/)
[AWS](../) | [AWS:IAM](../iam/)


- Install / Upgrade AWS CLI

1. Install Python3 PIP. On Ubuntu machines

```sudo apt-get install python3-pip```

2. Install / Upgrade AWS CLI via ```pip``` for the current user

```pip3 install awscli --upgrade --user```

  To install is for all users i.e., globally, remove the ```--user``` parameter.

3. Check your installed version

```aws --version```

To find-out the latest released version of [AWS CLI](https://github.com/aws/aws-cli/releases) go [here](https://github.com/aws/aws-cli/releases)

Also See: [AWS Documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
