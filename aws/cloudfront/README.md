### How to conifure AWS Cloudfront to access Private bucket


1. Create a bucket with no public access

2. Create a CloudFront Distribution

- Make sure to select `Yes` to the option `Restrict Bucket Access` and this will allow you to create `Origin Access Identity — OAI`. You may want to create new Identity and then `Yes` to `Grant Read Permission on Bucket`.

- You can see the OAIs created under **CloudFront — Security — Origin access identity**. The field ID will be similar to IAM user name and your bucket policy will reference to that. You can also create and ID manually here and then use it without creating an OAI when creating the CloudFront distribution. Note the ID is automatically generated when creating an OAI.

- Note that you can manually update the policy like above if you know the OAI ID and create the ARN as “arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity **OAI-ID**”

[Original Article](https://medium.com/@sameera.godakanda/using-cloudfront-to-allow-public-access-to-content-in-private-s3-bucket-2b01b041966e)

