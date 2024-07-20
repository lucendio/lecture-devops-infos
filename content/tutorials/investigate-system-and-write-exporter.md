---
title: 'Investigate system status & write exporter'
---


Investigate system status
=========================


## Objectives

* use standard tooling to investigate current system state
* investigate different aspects of a system helpful for troubleshooting (e.g. memory
  utilization, DNS resolution or port mapping)
* learn about the Prometheus metrics exposition [format](https://prometheus.io/docs/instrumenting/writing_exporters/#metrics) ([docs](https://github.com/prometheus/docs/blob/master/content/docs/instrumenting/exposition_formats.md#text-format-details))


## Prerequisites

* a running linux system (please refer to one of the previous tutorials)


## Tasks

1. Inspect the system via terminal 
    * look through logs
    * check current memory utilization
    * look up amount of free storage 
    * list processes running at the moment
    * check which ports are currently open
    * search for installed packages
2. Write a small program (exporter) in the language of your choice that exposes metrics in a 
   Prometheus-compatible format
    * choose 3 different metrics


## Deliverables

* source code of the executable exporter service


## Solution

*Please note that this solution is based on an Ubuntu 22.04 system. Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/investigate-system-and-write-exporter).*


### (0) Preparations

Spin up a linux machine, locally or in the cloud.


### (1) Investigate the system

{{< hint >}}
It's recommended to type out the following command and not copy&pate them. For further information
about a command, please refer to its documentation (e.g. man page: `man curl`, or the `--help` flag)
{{< /hint >}}


#### System information & resources

Linux kernel version:
```
uname -r
```

System information:
```
cat /etc/os-release
# OR 
hostnamectl
```

Processor information:
```
lscpu
```

Memory information:
```
lsmem
```

How long is the system already running:
```
uptime
```


#### Runtime information & utilization

Memory usage:
```
cat /proc/meminfo
# OR
free --human --wide    # [-hw]
```

CPU usage (`apt insatll -y sysstat`):
```
mpstat
# OR
iostat
```

Process monitor (cpu, memory, processes):
```
htop
```

I/O monitor - read/write disk (`apt insatll -y iotop`):
```
sudo iotop
```

Process tree:
```
pstree
# OR
ps -a -x -j -f
```

Check whether a process runs:
```
ps -a -u -x | grep nginx
```

Disk(s) storage utilization and file system type:
```
df --human-readable --print-type    # [-hT]
```

Storage utilization in a given path:
```
du --human-readable --all --max-depth 1 ./    # [-had 1]
```


#### Network & DNS

List all network interfaces:
```
ip a
```

Show default route:
```
ip default show
```

DNS lookup (forward & reverse - FQDN o.r IP):
```
nslookup 9.9.9.9
nslookup dns9.quad9.net
```

Look up DNS record with a given DNS server:
```
# MX: mail server
dig @9.9.9.9 MX mozilla.org
```

Show every process and the port(s) it is bound to for all protocols (and version):
```
netstat --program --listening --udp --numeric --tcp    # [-plunt]
# OR
ss --processes --extended --memory --listening   # [-peml]
```

Network bandwidth usage - read/write disk (`apt insatll -y iftop`):
```
sudo iftop
```

Network diagnostic:
```
mtr
```

Check whether a certain port is open:
```
nc -zv -w 4 github.com 22
```


#### Log inspection

```
journalctl
```

Boot logs:
```
journalctl -t kernel
# OR
dmesg
```

View logs from a certain process:
```
journalctl -u nginx
```

Follow log output:
```
journalctl --follow     # [-f]
```

View logs in a given time window from the end:
```
journalctl --since 'YYYY-MM-DD HH:MM' --until 'YYYY-MM-DD HH:MM'
# OR
journalctl -s '2 hours ago'
```

Export ALL logs:
```
journalctl > kernel.log
```


#### Debugging files

List open file system objects (sockets, files, directories, etc.) and which process has opened them: 
```
lsof
```

File information:
```
file $PATH
stat $PATH
```


#### Packages & Dependencies (focused on *Debian* and package manager `apt`)

Check whether a package is installed:
```
apt list | grep nginx
```

Search for a package:
```
apt search nginx
```

List package dependencies:
```
apt depends nginx
```

List all files a package has installed:

```
dpkg -L curl
```


### (2) Write a metrics exporter

1. Choose a language
    
    * Bash
    * Python 3

2. Choose at least three metrics

    * running processes
    * inbound traffic bandwidth
    * uptime
    * memory usage
    
3. Write the exporter

    * generate a multi-line string in *Exposition format*
    * start a web server
    * serve the string directly or from a file written to the filesystem beforehand


The result, 97 lines of code in a
[`exporter.sh` script](https://github.com/lucendio/lecture-devops-code/blob/master/tutorials/investigate-system-and-write-exporter/exporter.sh)
might do the trick.
