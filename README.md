RDSTail
=======

RDSTail is a tool for tailing or streaming RDS log files.  Supports piping to papertrail.
This is a fork from [litl/rdstail](https://github.com/litl/rdstail)

Changes are 
* Updated location of cli library and fixing a warning associated with newer version of said lib.
* RDS instance id can now be given as env var, mostly for ease of use with Docker.
* Polling tries made configurable, if there are no new loglines in (tries * rate) seconds rdstail goes to look for a newer log.
* Tries and rate set to more sensible defaults for a quiet PosgreSQL instance, only logging once every 5 minutes (10 * 30 == 300 seconds)
* Added Rockerfile (see: https://github.com/grammarly/rocker)

Installation
============

For now, you must compile from source.  Install [Go](https://golang.org).

    » go get github.com/madeddie/rdstail


Usage
=====

```
» rdstail -h

NAME:
   rdstail - Reads AWS RDS logs

    AWS credentials are taken from an ~/.aws/credentials file or the env vars AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY.

USAGE:
   rdstail [global options] command [command options] [arguments...]

VERSION:
   0.1.0

COMMANDS:
     papertrail  stream logs into papertrail
     watch       stream logs to stdout
     tail        tail the last N lines
     help, h     Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --instance value, -i value  name of the db instance in rds [required] [$RDS_INSTANCE]
   --region value              AWS region (default: "us-east-1") [$AWS_DEFAULT_REGION]
   --max-retries value         maximium number of retries for rds requests (default: 10)
   --help, -h                  show help
   --version, -v               print the version
   
------------------------------------------------------------
» rdstail papertrail -h

NAME:
   rdstail papertrail - stream logs into papertrail

USAGE:
   rdstail papertrail [command options] [arguments...]

OPTIONS:
   --papertrail value, -p value  papertrail host e.g. logs.papertrailapp.com:8888 [required]
   --app value, -a value         app name to send to papertrail (default: "rdstail")
   --hostname value              hostname of the client, sent to papertrail (default: "os.Hostname()")
   --rate value, -r value        rds log polling rate (default: "10s")
   --tries value, -t value       rds log polling retry rate (default: 30)
   
------------------------------------------------------------
» rdstail watch -h

NAME:
   rdstail watch - stream logs to stdout

USAGE:
   rdstail watch [command options] [arguments...]

OPTIONS:
   --rate value, -r value   rds log polling rate (default: "10s")
   --tries value, -t value  rds log polling retry rate (default: 30)
   
------------------------------------------------------------
» rdstail tail -h

NAME:
   rdstail tail - tail the last N lines

USAGE:
   rdstail tail [command options] [arguments...]

OPTIONS:
   --lines value, -n value  output the last n lines. use 0 for a full dump of the most recent file (default: 20)

```
