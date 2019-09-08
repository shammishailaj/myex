[Home](../) | [Alpine Linux](../alpine-linux) | [Docker](../docker) | [Docker-Compose](../docker/compose/)
- Installing S3CMD on Windows CF. [https://docs.cloud.datapipe.com/downloads/cloud-object-storage/setting-up-and-using-s3cmd/s3cmd-for-windows](https://docs.cloud.datapipe.com/downloads/cloud-object-storage/setting-up-and-using-s3cmd/s3cmd-for-windows)

1. Download and install python 2.x.  Python 3 is not supported. You can get it [HERE](https://www.python.org/downloads/windows/).
2. Add the python directory to your global PATH variable. E.g.,%SystemRoot%\system32;%SystemRoot%;c:\Python27
3. Download s3cmd from [www.s3tools.org](https://s3tools.org/download) or [Github](https://github.com/s3tools/s3cmd/archive/master.zip)
4. Open a command window, navigate to the folder to which you downloaded s3cmd and run ```python setup.py install```
5. From the command window, run python s3cmd --configure from the build path.
6. Use notepad to edit s3cfg.  When finished, it should look something like [THIS](https://docs.cloud.datapipe.com/cloud-object-storage/setting-up-and-using-s3cmd/s3cfg-example).
