# website osint
https://www.wappalyzer.com/ - technologies of website
https://archive.org/web/ - archive
git
git clone git@172.65.251.78:webitru_projects/p2p.git --config  core.sshCommand="ssh -i key"
## amazon cloud osint
http(s)://{name}.s3.amazonaws.com  One common automation method is by using the company name followed by common terms such as {name}-assets, {name}-www, {name}-public, {name}-private, etc.
site:*.dom.com
## subdomosint
https://crt.sh/?q=tryhackme.com
dnsrecon
Sublist3r
## virtual hosts
> ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP