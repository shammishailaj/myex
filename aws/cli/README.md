[Home](../../) | [Alpine Linux](../../alpine-linux/) | [Docker](../../docker/) | [S3CMD](../../s3cmd/) | [Go](../../go/)
[System](../../system/) | [Security](../../security/) | [MySQL](../../mysql/) | [InfluxDB](../../influxdb/)
[AWS](../../) | [AWS:IAM](../../iam/)
---
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


- Install Version 2.x [AWS Documentation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html#cliv2-linux-prereq)

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

- Update Version 2.x [AWS Documentation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html#cliv2-linux-prereq)

```
sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
```

[Migrating to AWS CLI v2.x](https://docs.aws.amazon.com/cli/latest/userguide/cliv2-migration.html#cliv2-migration-ecr-get-login)


### [AWS CLI Version 2.x ECR Login Migration](https://docs.aws.amazon.com/cli/latest/userguide/cliv2-migration.html#cliv2-migration-ecr-get-login)

- Version 1.x

```
$(aws ecr get-login -no-include-email)
```

- Version 2.x

```
aws ecr get-login-password | docker login --username AWS --password-stdin MY-REGISTRY-URL
```
