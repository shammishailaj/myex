1. To add environment variables

	i. Create a file with a _.sh_ extension inside /etc/profile.d/ and add commands to it. You will need to re-login to enable these.

	###### Cf. https://stackoverflow.com/a/35357011/6670698


2. To install PHP 7 and other packages that are reported as:
	ERROR: unsatisfiable constraints:
  		<package-name> (missing):
    		required by: world[<package-name>]

    i. ````apk add --update --repository http://dl-cdn.alpinelinux.org/alpine/edge/main --repository http://dl-cdn.alpinelinux.org/alpine/edge/community```

    ###### Cf. https://stackoverflow.com/a/39921218/6670698

3. 