[Home](../) | [Alpine Linux](../alpine-linux/) | [Docker](../docker/) | [S3CMD](../s3cmd/) | [Go](../go/) | [System](../system/) | [Security](../security/) | [MySQL](../mysql/) | [InfluxDB](../influxdb/) | [AWS](../)


- Example policy to provide minimal access to an S3 bucket to an application via IAM. The bucket the following policy provides access to is named ```cloud.example.com```

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "SRtmt1490776725000",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::cloud.example.com"
            ]
        },
        {
            "Sid": "SRtmt1490776852000",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:PutObjectACL"
            ],
            "Resource": [
                "arn:aws:s3:::cloud.example.com/*"
            ]
        },
        {
            "Sid": "SRtmt1490776725001",
            "Effect": "Allow",
            "Action": [
                "s3:ListAllMyBuckets"
            ],
            "Resource": [
                "arn:aws:s3:::*"
            ]
        }
    ]
}
```