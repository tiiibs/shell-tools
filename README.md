# shell-tools
collection of usefull shell commands and scripts
# One Line Commands
## Get the count of incoming tcp connections by host on a specific port
A one line style command to get the count of all connected hosts on specific port. As an example here I used the Postgres default port. It will also try to get the host name from /etc/hosts and get you the total of connections.
Useful to get a quick idea of connected hosts


```bash 
port=5432; allcount=0 ; for out in $(netstat -na | grep "$port" | awk ' {print $5}' | grep -oE "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b"  | grep -vE "(127|0)\.0\.0\.(1|0)" | sort |  uniq); do  count=$(netstat -na | grep "$out" | grep "$port" -c); host_n=$(grep "$out" /etc/hosts | awk '{ print $3}') ; echo  "$host_n $out : $count" ; allcount=$((allcount + count)) ; done; echo "Total : $allcount"
```

Output

    web1 192.168.1.101 : 16
    web2 192.168.1.102 : 8
    backend1 192.168.1.103 : 18
    frontend1 192.168.1.104 : 25
    backend2 192.168.1.111 : 6
    stat1 192.168.1.121 : 16
    stat2 192.168.1.122 : 8
    backend3 192.168.1.123 : 15
    frontend3 192.168.1.124 : 15
    backend4 192.168.1.131 : 6
    Total : 133

## Get the count of TCP connexions by state on a specific port 
A one line style command to get the count of tcp connections by state. This can be helpfull to quickly identify some network troubles like too many connections in TIME_WAIT, SYNC or CLOSE_WAIT states. 


```bash 
port=61616;for stat in $(netstat -na | grep ":$port " | awk '{ print $6}' | sort |uniq) ; do  echo -n "$stat: "  ;  netstat -na | grep ":$port " | grep "$stat" -c ;done;
```
Output

    ESTABLISHED: 164
    LISTEN: 1
    TIME_WAIT: 1
