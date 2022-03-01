# nmap

nmap -F ip # fast scan

nmap -p 1-65535 ip

nmap -Pn -p 1-65535 # Skip host discovery
nmap -sV ip probes open port to determine service or version
nmap -iL list_of_ips.txt
nmap -sT # scan only tcp
nmap -sU # scan only udp but require root 

nmap scripts
/usr/share/nmapscripts/*.nse

nmap -script <name_of_script> IP # running vulnerability script on IP

nmap -A -Pn IP # does OS detection.
nmap -O IP# detect OS

