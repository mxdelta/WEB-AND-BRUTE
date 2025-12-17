# скан вебморд
	 eyewitness -f domain_real.txt -d ILFREIGHT_subdomain_EyeWitness

# WEB-AND-BRUTE
Всегда проверяйте robots.txt, sitemap.xml и исходный код страницы перед началом перебора.

nuclei -target https://10.28.16.182

	#GOBUSTER

gobuster dir -u http://10.10.10.10 -w /usr/share/wordlists/dirb/common.txt -t 50 -x php,html,txt,js -o gobuster_scan.txt

# Базовая аутентификация
gobuster dir -u http://10.10.10.10 -w /usr/share/wordlists/dirb/common.txt -U admin -P password

# Рекурсивное сканирование (3 уровня)
gobuster dir -u http://10.10.10.10 -w /usr/share/wordlists/dirb/common.txt -r

# Скрыть статус 404
gobuster dir -u http://10.10.10.10 -w /usr/share/wordlists/dirb/common.txt -q

# Указать конкретные коды ответа
	gobuster dir -u http://10.10.10.10 -w /usr/share/wordlists/dirb/common.txt -s 200,301,302

# Базовый поиск поддоменов (DNS режим):

	gobuster dns -d inlanefreight.htb -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
	gobuster dns -d inlanefreight.htb -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -t 50 -r 8.8.8.8 -o subdomains_results.txt
# Базовый поиск VHOST:
	gobuster vhost -u http://inlanefreight.htb -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
	gobuster vhost -u http://inlanefreight.htb -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -t 30 --append-domain -o vhost_results.txt

#FFUF

	Поиск виртуальных хостов
	ffuf -w namelist.txt:FUZZ -u http://10.129.203.101/ -H 'Host:FUZZ.inlanefreight.local' -fs 15157
	Поиск директорий
	ffuf -u http://10.10.10.10/FUZZ -w /usr/share/wordlists/dirb/common.txt -t 50 -c -e .php,.html,.txt,.js -o ffuf_scan.json -of json

# Скрыть определенные размеры ответов
ffuf -u http://10.10.10.10/FUZZ -w /usr/share/wordlists/dirb/common.txt -fs 0 
# Показать только определенные размеры
ffuf -u http://10.10.10.10/FUZZ -w /usr/share/wordlists/dirb/common.txt -fw 100,200,500

# Фильтр по кодам статуса
ffuf -u http://10.10.10.10/FUZZ -w /usr/share/wordlists/dirb/common.txt -fc 404,403

# Перебор директорий и расширений
ffuf -u http://10.10.10.10/FUZZ/FUZ2Z -w /usr/share/wordlists/dirb/common.txt:FUZZ -w /usr/share/wordlists/common_extensions.txt:FUZ2Z

# * Trasfer DNS ZONE and DNS recon

		nslookup -type=SRV _ldap._tcp.dc._msdcs.dgg.tgg.zazpbom.ru 	(домен dgg.tgg.zazpbom.ru - поиск контроллеров домена)
		Общее перечисление SRV-записей с контроллера домена dc2.dgg.game.ru
  		nslookup -type=SRV _ldap._tcp.dc._msdcs.game.ru			(game.ru) домен

  		dig @10.10.11.5 freelancer.htb axfr		(dns-server   domain)
		dig @<ip> <домен> NS
  		Смотрим в DNS  в ptr записи:

  		dnsrecon -r 192.168.0.0/16  -n 192.168.2.11 - (dns server) 	(Обратный обход (Reverse Lookup) диапазона IP-адресов для поиска PTR-записей)

		dnsrecon -d bank.htb -a -n 192.168.2.37 -(dns server)	(Метод «прямого запроса»: Запрашивает у конкретного DNS-сервера полную копию его зоны для домена.)
		1) Смотрим в DNS  в ptr записи: dnsrecon -r 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 (но поочереди)

		sudo arp-scan --localnet 		(arp сканирование сети)

		 nslookup
		> server 10.10.10.248
		Default server: 10.10.10.248
		Address: 10.10.10.248#53
		> svc_int.intelligence.htb
		Server:         10.10.10.248
		Address:        10.10.10.248#53
*******************************************************
		vim /etc/hosts/ ----> 10.129.203.6 inlanefreight.htb
		subfinder -d inlanefreight.com -v
		git clone https://github.com/TheRook/subbrute.git >> /dev/null 2>&1
		cd subbrute
		echo "ns1.inlanefreight.com" > ./resolvers.txt
		./subbrute.py inlanefreight.com -s ./names.txt -r ./resolvers.txt

*******************************************************
		dnstool.py -u 'intelligence\Tiffany.Molina' -p NewIntelligenceCorpUser9876 			10.10.10.248 -a add -r web1 -d 10.10.14.58 -t A (создание ДНС записи в домене)



# Стандартные wordlists в Kali Linux

	/usr/share/wordlists/dirb/common.txt
	/usr/share/wordlists/dirb/big.txt
	/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
	/usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
	/usr/share/wordlists/SecLists/Discovery/Web-Content/raft-medium-directories.txt
	/usr/share/wordlists/metasploit/common_passwords.txt
# HASHCAT

	hashcat -m <hash_type> -a <attack_mode> <hash_file> <wordlist_or_mask>
	hashcat --example-hashes
	hashcat -m 0 -a 0 hashes.txt rockyou.txt
	hashcat -m 0 -a 3 hashes.txt ?u?l?l?l?l?l?l?l
	/home/whoami/tools/hashcat-7.1.2/hashcat.bin -a 0 -m 31300 clean_hashh.txt /usr/share/wordlists/rockyou.txt (hashcat для timeroasting)

# Hydra

	hydra -l root -P /usr/share/wordlists/common_passwords.txt ssh://192.168.1.100
	hydra -l admin -P passwords.txt 192.168.1.100 ssh -s 2222
	hydra -l admin -P passwords.txt 192.168.1.100 ssh -t 1 -w 30 -f (медленный режим)
