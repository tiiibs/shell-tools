# shell-tools
collection of usefull shell commands and scripts
# One Line Commands
## Get all connected hosts on a port 
A one line style command to get all connected hosts on specific port. As an example here I used the SQL port. It will also try to get the host name from /etc/hosts and get you the total of connections.
Useful to get a quick idea of connected hosts


```bash 
port=5432; allcount=0 ; for out in $(netstat -na | grep "$port" | awk ' {print $5}' | grep -oE "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b"  | grep -vE "(127|0)\.0\.0\.(1|0)" | sort |  uniq); do  count=$(netstat -na | grep "$out" | grep "$port" -c); host_n=$(grep "$out" /etc/hosts | awk '{ print $3}') ; echo  "$host_n $out : $count" ; allcount=$((allcount + count)) ; done; echo "Total : $allcount"
```
