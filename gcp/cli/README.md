[Home](../../) | [Alpine Linux](../../alpine-linux/) | [Docker](../../docker/) | [S3CMD](../../s3cmd/) | [Go](../../go/) |
[System](../../system/) | [Security](../../security/) | [MySQL](../../mysql/) | [InfluxDB](../../influxdb/)
[AWS](../../)

# Installing GCP CLI

## On Ubuntu 16.04 Xenial

Add the repository

```echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list```

Install pre-requisites

```sudo apt-get install apt-transport-https ca-certificates gnupg```

Add the GPG key for Google Cloud

```curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -```

Update the apt application cache and then install gcp cli

```sudo apt-get update && sudo apt-get install google-cloud-sdk```

Set-up the Gcloud CLI
```gcloud init```
