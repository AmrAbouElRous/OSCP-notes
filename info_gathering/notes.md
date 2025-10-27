┌──(amro㉿amro)-[~/Documents/oscp/info_gathering]
└─$ for ip in $(cat list.txt); do dig $ip.megacorpone.com A +noall +answer ;done
┌──(amro㉿amro)-[~/Documents/oscp/info_gathering]
└─$ for x in $(seq 200 254); do dig -x 51.222.169.$x +noall +answer; done

sudo git clone --depth 1 https://github.com/danielmiessler/SecLists.git /usr/local/share/seclists
┌──(amro㉿amro)-[~/Documents/oscp/info_gathering]
└─$ var="/usr/local/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt" 
                                                                                 
┌──(amro㉿amro)-[~/Documents/oscp/info_gathering]
└─$ for ip in $(cat $var); do dig $ip.megacorpone.com A +noall +answer ;done

There are ways to automate this with tools like 
**dnsrecons** Using dnsrecon to automate DNS enumeration

  -t, --type TYPE       Type of enumeration to perform.
                        Possible types:
                            std:      SOA, NS, A, AAAA, MX and SRV.
                            rvl:      Reverse lookup of a given CIDR or IP range.
                            brt:      Brute force domains and hosts using a given dictionary.
                            srv:      SRV records.
                            axfr:     Test all NS servers for a zone transfer.
                            bing:     Perform Bing search for subdomains and hosts.
                            yand:     Perform Yandex search for subdomains and hosts.
┌──(amro㉿amro)-[~/Documents/oscp/info_gathering]
└─$ dnsrecon -d megacorpone.com -t std 
# └─$ dnsrecon -d megacorpone.com -t brt  # will use a listfile in /usr/share/...
┌──(amro㉿amro)-[~/Documents/oscp/info_gathering]
└─$ dnsrecon -d megacorpone.com -D ./list.txt -t brt
┌──(amro㉿amro)-[~/Documents/oscp/info_gathering]
└─$ var="/usr/local/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt"
┌──(amro㉿amro)-[~/Documents/oscp/info_gathering]
└─$ dnsrecon -d megacorpone.com -D $var -t brt  

**dnsenum** Using dnsenum to automate DNS enumeration
┌──(amro㉿amro)-[~/Documents/oscp/info_gathering]
└─$ dnsenum megacorpone.com
#var="/usr/local/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt"
┌──(amro㉿amro)-[~/Documents/oscp/info_gathering]
└─$ dnsenum -f $var megacorpone.com
