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

	#FFUF

ffuf -u http://10.10.10.10/FUZZ -w /usr/share/wordlists/dirb/common.txt -t 50 -c -e .php,.html,.txt,.js -o ffuf_scan.json -of json

# Скрыть определенные размеры ответов
ffuf -u http://10.10.10.10/FUZZ -w /usr/share/wordlists/dirb/common.txt -fs 0 
# Показать только определенные размеры
ffuf -u http://10.10.10.10/FUZZ -w /usr/share/wordlists/dirb/common.txt -fw 100,200,500

# Фильтр по кодам статуса
ffuf -u http://10.10.10.10/FUZZ -w /usr/share/wordlists/dirb/common.txt -fc 404,403

# Перебор директорий и расширений
ffuf -u http://10.10.10.10/FUZZ/FUZ2Z -w /usr/share/wordlists/dirb/common.txt:FUZZ -w /usr/share/wordlists/common_extensions.txt:FUZ2Z

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
