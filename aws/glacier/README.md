- Using AWS Glacier on Windows

Download the free client [here](fastglacier-3-9-1.exe) or from its [website](https://fastglacier.com/download.aspx)

See: https://fastglacier.com/console-uploader.aspx

- Automate using FastGlacier's command-line uploader _glacier-con.exe_

Open the folder where you installed FastGlacier and find _glacier-con.exe_.

Its usage is shown below:

```
glacier-con.exe upload account[:password] local-file region-code vault/folder
```
---

Parameter details:

```account``` - account name you specified when adding an account using gui wizard

```password``` - optional password to decrypt account credentials (master password)

```local-file``` - path to the file or directory on your disk (wildcards allowed)

```region-code``` - amazon glacier region code, supported regions are: 

```
[region name]                        [region code]

US East (N. Virginia)                 us-east-1
US East (Ohio)                        us-east-2
US West (N. California)               us-west-1
US West (Oregon)                      us-west-2
Canada (Central)                   ca-central-1
EU (Ireland)                          eu-west-1
EU (London)                           eu-west-2
EU (Paris)                            eu-west-3
EU (Frankfurt)                     eu-central-1
Asia Pacific (Singapore)         ap-southeast-1
Asia Pacific (Sydney)            ap-southeast-2
Asia Pacific (Tokyo)             ap-northeast-1
Asia Pacific (Seoul)             ap-northeast-2
Asia Pacific (Mumbai)                ap-south-1
```

```vault/folder``` - amazon glacier vault name and folder


 Examples:

```
glacier-con.exe upload my-account c:\backup us-east-1 my-vault/backups
```

```
glacier-con.exe upload my-account c:\backup\*.bkf eu-west-1 my-vault
```

```
glacier-con.exe upload my-account "e:\my videos" eu-west-1 "vault-3/my videos"
```

```
glacier-con.exe upload encrypted-account:85sd4df F:\Docs eu-west-1 Vault-4/Docs
```

**IMPORTANT:** If spaces appear in the path, enclose it in quotation marks. Do not use traling backslash.
