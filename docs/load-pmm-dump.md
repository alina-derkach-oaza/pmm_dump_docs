# load-pmm-dump

The utility `load-pmm-dump` creates empty PMM instance and prints commands which you can use to load the dump into test PMM instance, redirect PMM port to another machine, and destroy docker PMM container after use.

There are three ways to run the script. All of them require at least one parameter: the container name string.

``` {.bash data-prompt="$" }
./load-pmm-dump CS0012345
```

This will assume latest PMM server version 2.26.0, and will deploy two containers: pmm-data-CS0012345 and pmm-server-CS0012345.

``` {.bash data-prompt="$" }
./load-pmm-dump CS0012345 2.26.0
```

This will get the PMM server Docker version tag from the `.tar.gz` file metadata, and it will use it for the containers.

``` {.bash data-prompt="$" }
./load-pmm-dump CS0012345 /path/to/pmm-dump-1653879141.tar.gz
```

After it's done, the tool will output some helpful information and copy/paste-ready commands. For example:

```
## USEFUL INFORMATION AND COMMANDS.

## Port 443 is exported to:  49196

## Use the following for port redirection from your local machine:
ssh -L 8443:127.0.0.1:49196  highram

## To import a PMM dump:
pmm-dump import --allow-insecure-certs --pmm-url=https://admin:admin@127.0.0.1:49195 --dump-path=/bigdisk/agustin/load-pmm-dump-files/pmm-dump-1653879141.tar.gz

## Use the following to get human readable dates from a Unix timestamp:
date -d @1657948064

## Increase 'Data Retention' in the advanced settings if the samples are older than the default 30 days.

## To destroy docker containers:
docker rm -vf pmm-data-CS0027251 pmm-server-CS0027251
```
