**Tagy:** #
**Link:** [[Examining Newly Registered Phishing Domains at Scale.pdf]]

---

## Shrnutí

- Domény pro phishing buď nové nebo ukradnuté
- 15 126 nově registrovaných phishing domén
- 79 % nových domén má TLS certifikát
- SMS nedovolá obfuskaci URL, posílá se celá URL
- Stránky se dají blokovat pomocí geolokace
- Může se poslat phishing stránka jen na mobil a na PC být normální
- Primární dataset je daily feed od Cyber Defence Alliance
	- 34 TLS CAs
	- 190 TLDs
	- 58 brands
	![[Pasted image 20261022001643.png]]
- Comparison dataset
![[Pasted image 20261022001652.png]]
- Enrichment dat
	- WHOIS
	- ipinfo.io
	- crt.sh
	- Passive DNS
	- VirusTotal
- 190 z 1440+ TLDs bylo napadeno
- Dominuje .com s 55.2%
	- .com má podle W3Techs podíl 46%
	![[Pasted image 20261022001957.png]]
- Pomohlo by zvýšit ceny za .com ale to by zasáhlo i normální uživatele
![[Pasted image 20261101145714.png]]
- DomainTools ukazuje kolik domén existuje
- gTLD jsou generic jako .com, .org, .net atd...
- ccTLD jsou .cz, .sk atd...
- Nejvíce jsou zasaženy gTLD, nicméně pro moji práci by bylo fajn se podívat na ccTLD, nic pro analýzu .cz domén jsem nenašel
![[Pasted image 20261101151453.png]]
- Přes 226 WHOIS nevrátilo registrátora
- 109570 certifikátu pro 11996 domén v datasetu
	- 91 issuer IDs
	- 34 organizací
- Největší issuer je Let's Encrypt
	- Shoduje se se zbytkem webů na internetu
- Certifikáty se často používají i na subdomény
	- 1 certifikát může mít až tisíce subdomén
	- Extrémní chování
	- Je třeba kontrolovat i subdomény
![[Pasted image 20261101151810.png]]![[Pasted image 20261101153318.png]]
- Útočí se často na finanční instituce, na ty by bylo dobré se zaměřit
![[Pasted image 20261101153712.png]]
- VirusTotal spouští několik antivirů pro detekci a u přes 80% všech domén označil jako malicious jen 1 
	- Nedá se na VirusTotal spolehnout
- Google Safe Browsing má podobné výsledky
	- Je součástí VirusTotal
- Antivirová detekce pro nové domény není spolehlivá
- Měření životnosti domén je měřeno od nahlášení od CDA (nikoliv registrace) do dne kdy passive DNS Spamhause viděl překlad naposled
- 0 až 449 dní
- Medián je 1 den
- Průměr 8,6 dne
- 89,45 % žilo méně než 2 dny
- 6,59 % déle než 15 dní
- CDA posílá data i mobilním operátorům a ty je blokují, proto je malá životnost
- Přes 14 % domén je znovupoužito během 1 roku od takedownu
- 