**Tagy:** #detekce #klasifikace #dataset
**Link:** [[An effective approach to typosquatting detection.pdf]]

---

## Shrnutí

- Mezi 10 a 20 % ručně napsaných url má v sobě chybu
- 1 ze 14 uživatelů skončí na typosquatted doméně
- Metoda detekce není založena na ML
- 95 % typosquat URLS cílí na méně populární domény
- missing-dot, character-omission, character permutation, character-substitution a character-duplication
- Pro generování se používá Damerau-Levenshtein vzdálenost, rozšíření Levenstheinovi vzdálenosti, která přidává permutaci, 99 % (2016) domén registruje nějakou Dameru-Levenshtein doménu
- Je třeba rozlišit typosquatting od podobných domén, např. nhl.com a nfl.com
![[Pasted image 20261011184911.png]]
- Prefiltering
	- Kontrola zda je DN nebo VD v blacklistu
		- BlackBook list
	- Kontrola zda je DN ve whitelistu
		- kombinace Top Domain Repository a uživatelského whitelistu (User Domain Repository)
		- TDR je dynamický list 1000 nejvíce navštěvovaných stránek globálně a 1000 pro každý stát ([Top 1000 Websites By Ranking Keywords – DataForSEO](https://dataforseo.com/free-seo-stats/top-1000-websites))
	- Kontrola zda je green typo
		- defensivní domény
		- např. feceboook redirectuje na facebook
		- z DN se vypočítá VD
			- follow_forward a uloží se finální doména, na kterou vedou redirecty
				- pokud se vrátí 302, vezme se z headeru redirect a rekurzivně se volá
			- VN se následně zkontroluje ve whitelistu
	 - Pokud je DN nebo VD v blaclistu, bere se stránka jako nebezpečná, pokud ve whitelistu tak naopak
	![[Pasted image 20261011190857.png]]

- Feature extraction
	- Pouze pokud prefiltering nevrátil 1 pro blacklist nebo whitelist
	- Candidate Target Extraction
		- Parking Alert (PARKA)
			- Parking domain je registrovaná ale aktivně se nepoužívá, buď má placeholder page a nebo message, že se v budoucnosti používat bude
			- PARKA funguje na základě multi-language analýzy key-phrases typických pro parked domény
			- Kontroluje se jestli je doména known hosting resource pro parkovací domény
			- Pro implementaci se dá použít neuronová síť a screenshot, nicméně i jednodušší metody vrací satisfactory výsledky
				- Kontrola textu vůči manuálně curated datasetu typických frází
				- Kontrola zda je doména na známém providerovi parkovacích domén (godaddy.com)
				- Je to rychlejší než AI ale vrací více false negatives
					- Custom landing page, client rendered stránka, no machine-readable text, primárně obrázky, hosting mimo list známých providerů
		- Candidate Target
			- Sestaví se pole candidate targets (CT), domén co mají vzdálenost přesně 1
				- Whitelist z prefilteringu
		- Top 10 (T10A)
			- Top 10 kandidátů vyhledávače při hledání DN
			- Přidá se do pole CT, pokud mají vzdálenost 1
		- Did you mean?
			-  návrh vyhledávače, pokud existuje
			- V češtině to je "Měl jste na mysli?", co ukáže Google při špatně napsaném dotazu
			- Pokud má DYM vzdálenost 1, přidá se do CT
	![[Pasted image 20261011201148.png]]

	- Features Extraction
		- Candidate Target Alert (CTA)
			- 1 pokud CTA není prázdné pole
		- Top 10 Alert (T10A)
			- od -1 do 1
				- 1 pokud je CT první a DN ve výsledku není
				- 0 pokud jsou na stejné pozici
				- -1 pokud je DN první a CT ve výsledku není
			- Pro každou položku CT
			- Sestaví se nová tabulka se skóre CT
				- Rank je sestaví jako 0.1 * (rank(dn) - rank(ct))
					- Rank je pořadí v Top 10 vyhledávání
					- Pokud není v top 10, tak je rank 10
		- Did You Mean Alert (DYMA)
			- V googlu "Měl jste na mysli?" oprava
			- 1 pokud DYM = CT, jinak 0
		- Prochází se CT a pro každý prvek se počítá T10A a DYMA
			- vypočítá se součet T10A a DYMA, pokud je větší než max, tak se současné T10A a DYMA uloží
	![[Pasted image 20261012175110.png]]

- Klasifikace
	- 0 pokud je whitelisted nebo green typo
	- 7 pokud je blacklisted
	- Jinak 2 + PARKA v CTA +T10A + DYMA
	- Výsledná hodnota se porovná s hodnotou TH a pokud je AV vyšší, jedná se o typo
		- Ta byla ve studii nastavena na 2.1
	![[Pasted image 20261012175323.png]]

- Evaluation
	![[Pasted image 20261012203334.png]]
	- Dataset je postavený podle stejného pravidla jako detektor, vždy jen DL 1 on populárního webu
	- Žádný combo-squatting
	- Ruční ověření, to typo třídy šly jen domény na kterých se shodli všichni autoři