**Tagy:** #defense
**Link:** [[Shielding Brands An In-depth Analysis]]

---

## Shrnutí
- Organizace musí monitorovat domény, hlídat jejich expirační data a mít aktuální kontaktní info, aby je následně získali
- Studie zahrnuje 370 značek
- Firmy jako MarkMonitor hlídají domény, aby se mohli registrovat dříve, než je registruje útočník
- e2LD - effective second-level label
	- google.com = google
	- product.domain.co.uk = domain
- TLD swap
	- seznam.cz -> seznam.com
- dataset ze srpna 2023, obsahuje několik po sobě navazujících operací
	- zkompilovali set značek často pod útokem, Amazon atd...
		- Ze 3. blocklist service, APWG, PhishTank a OpenPhish
		- 370 značek
	- Vezme dataset a vygenerují e2LL
		- dnstwist
			- používají:
				- typosquatting
					- addition, insertion, omission, repetition, replacement, vowel-swap a transposition
				- www+brandname
				- homoglyph
					- jiné abecedy podobné latince
				- bitssquatting
					- abuse rare bit errorů
				- homophones
		- dataset rozšířili o combo squatting se 179 slovy 
	- Passive DNS data
		- 208 NOERROR a 221 NXDOMAIN
		- pDNS real time data stream
		- Provede se WHOIS scan
	- WHOIS
		- k doménám se přídá registrátor, IANA ID a datum vytvoření
	- Defenzivní registrace
		- vyfiltruje domény od 11 defenzivních registrátorů
	- Sunrise period
		- datum vytvoření z WHOIS porovnávají s daty fází od ICANN
		- přiřadí každé ngLTD doméně
			- ngLTD jsou domény z roku 2013, kde se mohli registrovat nové domény
			- .app, .paris atd....
				- např. možný útok amazon.shop
		- Labely
			- Sunrise - smí registrovat jen držetelé ochranné známky
				- Fáze trvá jen 60 dní
				- O sporné domény rozhoduje aukce
			- General Availability - registrovat může kdokoli
			- Claims - prvních 90 dní general availability
				- Kdo registruje doménu shodnou se zapsanou známkou, dostane varování
				- Pokud ji přesto registruje, TMCH upozorní vlastníka značky
- Druhým datasetem jsou domény UDRP
	- Mimosoudní rozhodčí
	- Primárně 4 centra
	- Braly jen spory co byly vyřízené a starší než 30 dní
	![[Pasted image 20261014194039.png]]![[Pasted image 20261014201120.png]]
- 36 027 defenzivních domén, z toho 30 138 pod ngTLDs
- Nejvíce se registruje combosquatting
- Značky registrují domény pod novými TLD pozdě
	- 68,7% až v době, kdy je mohl koupit každý
- Pozdní registrace vedli k 3145 sporům
![[Pasted image 20261014201822.png]]
![[Pasted image 20261014201937.png]]