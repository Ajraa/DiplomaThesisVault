**Tagy:** #cloaking
**Link:** [[On cloaking behaviors of malicious websites.pdf]]

---

## Shrnutí

- Samarasinghe & Mannan, Computers & Security 101 (2021), data z roku 2019
- Cloaking = škodlivý web ukáže crawleru (Googlebot) neškodný obsah a uživateli podvod
	- cíl: vyhnout se blacklistům a odhalení
- Dataset: 100 000 registrovaných squatting domén
	- 92 200 typosquatting domén
		- dnstwist, seed = top 1 983 webů z Tranco
		- addition, bitsquatting, homoglyph, hyphenation, insertion, omission, repetition, replacement, sub-domain, transposition, vowel-swap
	- 7 800 combosquatting domén (např. support-apple.com-identify.us)
		- z certifikátů (Censys, CertStream = CT logy) a DNS záznamů (Project Sonar)
		- vyfiltrované ML nástrojem SquatPhish
- Metoda
	- crawler (Puppeteer, headless Chrome) načte každou stránku 2× jako Chrome a 2× jako Googlebot
		- liší se jen hlavička User-Agent
	- porovnává se:
		- hlavičky (title, description, keywords)
		- odkazy
		- text stránky - SimHash, hammingova vzdálenost > 20 = podezřelé
		- téma stránky - LDA
		- screenshoty - ImageHash + OCR
- Výsledky
	- cloaking dělalo 3 880 domén (3,9 %)
	- 80 % cloakovaných webů je škodlivých, VirusTotal jich označil jen 3,3 %
	- ze všech 100 000 domén VirusTotal označil 2,3 %, škodlivých je podle autorů 74 %
	- po ~3 měsících pořád ~46 % domén cloakovalo a na blacklistech skoro nebyly
		- => blacklisty proti cloakingu nefungují
	- obsah: falešná technická podpora ("your computer was locked"), loterie a výhry, škodlivá rozšíření prohlížeče, stránky s reklamními odkazy
	- cloaking je častější:
		- u combosquattingu než typosquattingu
		- přes HTTP než HTTPS
		- na mobilu než na desktopu (797 vs. 607 statických webů)
	- nejvíc cloakovaných domén registroval GoDaddy (477), hostované hlavně v USA

## Metody, které stránky využívají
- **Cloaking podle User-Agentu** - nejčastější
	- server přečte User-Agent, Googlebot dostane neškodnou stránku, Chrome podvod
	- u statických stránek 11-16× častější než podle refereru
- Cloaking podle refereru
	- podvod se ukáže jen tomu, kdo přišel z výsledků vyhledávání (Referer: google.com)
- Cloaking podle IP adresy
	- pozná IP rozsahy crawlerů, v článku popsán, ale neměřen
- Repeat cloaking
	- podvod jen při první návštěvě, pak se pozná podle cookie nebo IP
- Falešné chyby pro crawler
	- Googlebot dostane 404, 403 nebo "Coming soon", uživatel skutečnou stránku
- Dynamický obsah
	- stránka náhodně střídá neškodný a škodlivý obsah
	- škodlivé odkazy generované JavaScriptem v iframe
- Skrývání odkazů před crawlerem
	- Chrome viděl víc než 2× tolik odkazů (21 616 vs. 10 355)
- Klíčová slova jen pro crawler (semantic cloaking)
	- 716 webů mělo keywords (health, surgery...) jen pro Googlebot, kvůli SEO
- Přesměrování přes další domény
	- 1,8 % cloakovaných domén, např. plt2t.com -> scam s falešnou podporou

## Pro diplomku
- Argument, že blacklisty nestačí -> smysl heuristické analýzy URL
- Falešné pozitivy
	- parkované domény na prodej (GoDaddy, Sedo)
	- přesměrování na legitimní web = obranná registrace
	- stejný vlastník ve WHOIS jako značka (našli jen expesdia.com)
	- WHOIS s "REDACTED FOR PRIVACY" nebo Domains by Proxy vlastníka neprozradí
- Rozšíření: stáhnout stránku s různými User-Agenty a porovnat, sledovat CT logy
- Pozor
	- data jsou předfiltrovaná na "spíš škodlivé"
	- ground truth stojí na Symantec SiteReview
	- čísla si v článku odporují (22 % vs. 80 %, 37,4 % vs. 74 %), citovat konkrétní sekci
