# go-cpe-dictionary

This is tool to build a local copy of the CPE (Common Platform Enumeration) [1].

> CPE is a structured naming scheme for information technology systems, software, and packages. Based upon the generic syntax for Uniform Resource Identifiers (URI), CPE includes a formal name format, a method for checking names against a system, and a description format for binding text and tests to a name.

go-cpe-dictionary download CPE data from NVD (National Vulnerabilities Database) [2].
Copy is generated in sqlite format.

[1] https://nvd.nist.gov/cpe.cfm  
[2] https://en.wikipedia.org/wiki/National_Vulnerability_Database  

[![asciicast](https://asciinema.org/a/asvc87lbpad5999shqk0xvtc0.png)](https://asciinema.org/a/asvc87lbpad5999shqk0xvtc0)

## Install requirements

go-cpe-dictionary requires the following packages.

- sqlite
- git
- gcc
- go v1.6
    - https://golang.org/doc/install

```bash
$ sudo yum -y install sqlite git gcc
$ wget https://storage.googleapis.com/golang/go1.6.linux-amd64.tar.gz
$ sudo tar -C /usr/local -xzf go1.6.linux-amd64.tar.gz
$ mkdir $HOME/go
```
Put these lines into /etc/profile.d/goenv.sh

```bash
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

Set the OS environment variable to current shell
```bash
$ source /etc/profile.d/goenv.sh
```

## Deploy go-cpe-dictionary

To install, use `go get`:

go get

```bash
$ sudo mkdir /var/log/vuls
$ sudo chown ec2-user /var/log/vuls
$ sudo chmod 700 /var/log/vuls
$ go get github.com/kotakanbe/go-cpe-dictionary
```

Fetch CPE data from NVD. It takes about 1 minutes.  

```bash
$ go-cpe-dictionary fetch
... snip ...
$ ls -alh cpe.sqlite3
-rw-r--r-- 1 ec2-user ec2-user 7.0M Mar 24 13:20 cpe.sqlite3
```

Now we have a local copy of CPE data in sqlite3.  

# How to search CPE name by application name

This example use [Peco](https://github.com/peco/peco) for incremental search.

```
$ ls cpe.db
cpe.db
$ sqlite3 ./cpe.db 'select name from cpes' | peco
```

[![asciicast](https://asciinema.org/a/asvc87lbpad5999shqk0xvtc0.png)](https://asciinema.org/a/asvc87lbpad5999shqk0xvtc0)


# Usage:

```
$ go-cpe-dictionary -help
Usage of ./go-cpe-dictionary:
  -dbpath string
        /path/to/sqlite3/datafile (default "/Users/kotakanbe/go/src/github.com/kotakanbe/go-cpe-dictionary/cpe.db")
  -dump-path string
        /path/to/dump.json (default "/Users/kotakanbe/go/src/github.com/kotakanbe/go-cpe-dictionary/cpe.json")
  -fetch
        Fetch CPE data from NVD
  -http-proxy string
        HTTP Proxy URL (http://proxy-server:8080)
  -load
        load CPE data from dumpfile
  -v    Debug mode
  -vv
        SQL debug mode
```

----

# Misc

- HTTP Proxy Support  
If your system is behind HTTP proxy, you have to specify --http-proxy option.

- How to cross compile
    ```bash
    $ cd /path/to/your/local-git-reporsitory/go-cpe-dictionary
    $ GOOS=linux GOARCH=amd64 go build -o cvedict.amd64
    ```

- Debug  
Run with --debug, --sql-debug option.

----

# Data Source

- [NVD](https://nvd.nist.gov/)

----

# Authors

kotakanbe ([@kotakanbe](https://twitter.com/kotakanbe)) created go-cpe-dictionary and [these fine people](https://github.com/future-architect/go-cpe-dictionary/graphs/contributors) have contributed.

----

# Contribute

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

----

# Change Log

Please see [CHANGELOG](https://github.com/kotakanbe/go-cpe-dictionary/blob/master/CHANGELOG.md).

----

# Licence

Please see [LICENSE](https://github.com/kotakanbe/go-cpe-dictionary/blob/master/LICENSE).

----

# Additional License

- [NVD](https://nvd.nist.gov/faq)
>How can my organization use the NVD data within our own products and services?  
> All NVD data is freely available from our XML Data Feeds. There are no fees, licensing restrictions, or even a requirement to register. All NIST publications are available in the public domain according to Title 17 of the United States Code. Acknowledgment of the NVD  when using our information is appreciated. In addition, please email nvd@nist.gov to let us know how the information is being used.  
 
