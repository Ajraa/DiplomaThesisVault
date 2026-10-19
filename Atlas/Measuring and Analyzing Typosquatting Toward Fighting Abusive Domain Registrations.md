**Tagy:** #
**Link:** [[Measuring and Analyzing Typosquatting Toward Fighting Abusive Domain Registrations.pdf]]

---

## Shrnutí

- Stará data z roku 2013
	- Alexa byl žebříček nejnavštěvovanějších webů od Amazonu
		- top, 1 až 10 000 místo
		- mid, 10 000 až 250 000 místo
		- tail 250 000 až 1 000 000, dlouhý ocas
- Nejčastější jsou DL1 domény
	- Demerau-Levenshtein vzdálenost 1
- Monetizace
	- third party ads
	- phishing
	- malware
		- drive-by download
	- redirekce na landing doménu
	- redirekce ke kompetici
	- redirekce s affiliate linkem
	- defenzivní registrace
	- pay per click a par per redirect
- Postupem času přibývá cloaking
	- web může detekovat crawler a ukázat validní stránku
- Typosquatting není nelegální
- Pouze necelých 7% cílí na top 10 000 .com domén
	- 2013, nicméně [[An effective approach to typosquatting detection]] došel k podobnému závěru
- Jen 2% webů přesměrují na validní web, většina jsou pak parkovací stránky
	- defenzivní registrace
- gtypo = generated typo domain
	- DL1
- ctypo = candidate typo domains
	- Podmnožina gtypo
	- skutečně registrované domény
	- ctypo není nezbytně typosquatting
- delší ctypo domény mají větší šanci na typosquat
- T - typosquatting, NT - not typosquatting, U - uknown (http/dns errory)
- P - parker, PA - parked with ads, PH - phishing, M - malware, D - defensive, A - affiliate, O - other redirect
![[Pasted image 20261012232048.png]]
![[Pasted image 20261012231911.png]]
- ALLTYPO
	- každý DL1 doména je typo
- SUT
	- rozhoduje podle html
		- velikost stránky
		- pop-upy
		- klíčová slova
- YATT-P
	- jen offline data
- YATT-PD
	- DNS a WHOIS
- YATT Pomocí regexů měl 85 % predikci na parked domény
	- Na defenzivní domény moc nefungovali
- Legální akce jsou drahé
	- Těžce prokazatelná zlá víra
	- Firmy jsou zahlcené
- Typosquatting vede i k phishingu a scamům
- Blacklisty nejsou moc spolehlivé
- WHOIS kvůli GDPR nejsou spolehlivé
- Typosquatting platí i pro maily, např pepa@gmil.com
- 