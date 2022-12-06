##  Suricata monitoring
`tail -f /var/log/suricata/eve.json | jq -r -c 'select(.event_type=="alert")'`

## Target specification:
* scanme.nmap.org/16 192.168.1.2/24 10.20.1-16.1-254
* 192.168.10.8 10.0.1,3,5,8.1-255
* -iL <file> (space, tabs or newlines) or "-iL -" to get from STDIN
* --exclude comma separeted list
*  --excludefile <file>

## Reverse DNS resolution
* -n no resolution
* -R resolve eve down hosts


## Ping Sweep

* nmap -sn <target>
* masscan -p1-65535 --banners --http-user-agent "Mozilla/5.0 Firefox/42.0" --open-only 192.168.0.1
* masscan -pU:5060-5061 --ping --banners --open-only

## General options
* -sL list targets to confirm ip addresses
* -PN skip ping
* -PE ICMP echo, timestamp and netmask request
* -PP Timestamp
* -PM Address mask
* -PA Ack ping
* -sP -n just host discovery
* -sP -PE icmp ping only
* -sP -PS22,80,443 -R TCP SYN ping
* -sP -PA80,21,110 TCP ACK ping
* -sP -PU53,38,466 UDP ping (to closed ports, default 31,338)
* -sP -PP ICMP timestamp
* -sP -PM ICMP address mask
* -sP -PO IP protocol ping
* -sP -PR ARP scan
* --source-port 53 useful for UDP/TCP pings
* --data-lenght 32 to evade IDS with rules for zero lenght pings
```
              32 Windows echo request
              56 Linux default ping
```
* --max-parallelism defaults to the number of targets
* --randomize-hosts to further evade sensors
* --reason expands on why the hosts is up
* -D <decoy1,decoy2,...>
```
Ex:
  nmap -sP -oX np_ips.xml 204.225.42.0/23
  nmap -sP -PE -PP -PS21,22,25,80,113,8080 -PA80113,443,10050 -T4 --source-port 53
```

## Port scanning
By default scans the most popular 1.000 ports of the specified protocol.

* -A remote OS detection
* -p0- scan all ports
* -sS TCP Syn stealth
* -sT TCP Connect
* -sU UDP
* -sF TCP FIN
* -sX TCP XMASS
* -sN TCP NULL
* -sA TCP Ack - map firewall rulesets
* -sW TCP Window - like ACK, but detects openports agains some machines
* -sM Maimon - firewall-evading similar to FIN, works with a few systems
*-sI TCP Idle
* -sO -p <protocol> - Protocol scan
* -F (fast) - scans only 100 most popular ports.
* --top-ports <num> - specify number
* k-p <list of ports> (pg. 84)
* -T0 Very slow | -T5 extremely aggressive
  
```
Ex: 
  nmap -T4 -PN -p80 --max-rtt-timeout 200 --initial-rtt-timeout 150 --min-hostgroup 512 -oG losg/openport80-%D.gmap 200.1.10.0/24
     %D - Numeric date

  ```
## Version detection
* -sV
* -A - Advanced and aggressive features

```  
Ex:
nmap -sV --scan-delay 1 -sT -p<ports>
```

## OS detection
* -O
* -v - verbose

## Traceroute
* --traceroute
  
## Script scanning
* -sC
* --script - to specify a custom set os scripts
* --script-args - arguments to scripts
* --script-trace - debug script

```
Ex:
nmap -sC --script-args user=foo,pass=sapeca,whois={whodb=nofollow+ripe}
nmap --script=./showSSHVersion --script-trace example.com
nmap --script=mycuston,safe,discovery example.com
```
  
Categories
* auth
* default
* discovery
* external
* intrusive
* malware
* safe
* version
* vuln
  
Bypass Snort
* Connect Scan: -sT
* Apenas uma porta: -p80 ou -p443
  
Bypass Suricata
* Alterar user-agent
* Alterar strings 'sqlspider' no script
  
```
Ex:
nmap --scan-delay 1 -sT -p80 --script=http-sql-injection 192.168.0.1 --script-args http.useragent="Mozilla/5.0"
```
  
## Subverting Firewalls and IDSs
* pg 260. Nmap Network Scanning
