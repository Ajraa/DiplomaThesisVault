**Tagy:** #detekce #klasifikace
**Link:** [[DomainLynx Leveraging Large Language Models.pdf]]

---

## Shrnutí

- Autoři chtějí, aby se citovala rozšířená verze v IEEE Access 2025.
- Detekce pomocí LLM
	- LLama-3-70B
- Sound-squatting
	- slova co zní podobně eggsample -> example
- Level-squatting
	- doména je v subdoméně
		- example.com.comain.example.com
- Hybrid-squatting
	- Nové v této studii
	- Kombinace několika způsobů
- 4 hlavní komponenty
	- Data processing
		- Přijímá data z několika zdrojů
	- Domain Name eXpansion (DNX)
		- Identifikuje squatted domény s jejich legit protějškem pomocí vektorové databáze
		- Embedding-based párování pomocí OpenAI text-embedding-3-small
		- Podobnost místo string-based matchingu
		- Využívá Weaviate
			- open-source vector databáze
			- Embedding top 100 000 domén z Tranco listu
		- Implementuje Retrieval Augmented Generation (RAG) pro domain pairing
		- Vektorový search který pro doménu vyhledá nejpodobnější doménu v databázi
		- Hledáním vznikne pár typo domény s legitimní doménou
		- Páry se abecedně seřadí podle domén v databázi
			- Zde předpokládám, že hledali několik typo domén najednou
		- Výsledek se rozdělí na chunky po 100 a posílá do TRV
			- Chunk kvůli cost efektivitě pro LLM
	- Threat Recognition Validation (TRV)
		- Využívá LLM pro evaluaci
		- Must-Pass Domain Injection
			- Selekce
				- TRV injectne set jednoznačných domén, které by měl každý systém vyhodnotit na 100%, jak typo tak legit
				- amazon, google atd...
				- amzaon atd...
			- Umístění
				- Inject to všech chunkl
				- 4 must pass domény jsou umístěny na 26., 51., 76. a 101. místo
					- Mitigace lost in the middle fenoménu, kde LLM přehlíží center velkých inputů
			- Validace
				- Validace pak ověřuje LLM klasifikace must pass domén
					- Chyba pak triggerne reavaluaci celého chunku
		- Využívá JSON
				- s: subdomain, d: domain, sx: suffix
				- www.example.co.jp = {s: www, d: example, sx: co.jp}
		- Evaluace využívá jak dynamických referencí (Tranco top 100k), tak i knowledge base modelu z trénování
		- TRV má 4 kroky validačního procedu
			1. JSON formát validace
			2. Input-Output konzistence
				- Kontrola zda output korespenduje k inputu
				- Kvůli halucinacím, které můžou generovat domény, co nebyly na inputu
			3. Must Pass Domain verifikace
				- Zda must pass domény byly správně vyhodnoceny
			4. Legitimate Domain Existence verifikace
				- Ověřuje, zda cílová legitimní doména (pole l), kterou LLM uvedlo, opravdu existuje
				- LLM může cíl vzít i ze svých znalostí, nemusí být z Tranco
				- Cíl je v Tranco top 100k -> ověřeno
				- Není -> existence se ověří vyhledávačem
				- Neexistuje -> halucinace, výsledek se zamítne a chunk se spustí znovu
		![[Pasted image 20261018161154.png]]
	- Output generation
		- Generuje list potencionálních squatting domén
		- Iterativní validační proces
			- Spustí se reevaluace chunků, co fainuli
			- Určitý počet pokusů
		- 

	![[Pasted image 20261017194834.png]]

- Input data sources
	- Certificate Transparency (CT) logy
		- Log SSL/TLS certifikátů od trusted autorit
		- Pro objevení nově zaregistrovaných domén a subdomén
	- Passive DNS (pDNS) Records
		- Monitoring DNS query odpovědí
		- Useful pro subdomény
	- Centralized Zone Data Service (CZDS) Files
		- Soubor informací o registrovaných doménách ve specifických TLD

- Vyhodnocení
	- Experimentální setup
		- Ground Truth Dataset
			- 1649 domén cílících na Tranco top 1k, které byly flagnuty aspoň 5 tooly
				- Typo-squatting: 369
				- Homograph-squatting: 249
				- Bit-squatting: 136
				- Sound-squatting: 33
				- TLD-squatting: 156
				- Combo-squatting: 706
			- Má labely
		- Real-time Dataset
			- FQDNs za 1 měsíc
			- Nově pozorované, období 1.–31. 1. 2024
			- Zdroje: všechny veřejné CT logy, pDNS z 60+ DNS serverů, CZDS z 1 430 TLD
			- Filtr: jen FQDN s NS a zároveň A/AAAA záznamem (aktivně používané)
			- 2 099 184 FQDN
			- Bez labelů
				- Neví se předem zda jsou squatting nebo ne
		- Datasety nejsou spojené
		- GPT-3.5, GPT-4o a Llama-3-70B
		- Porovnání vůči 2 pravidlovým metodám z jiných prací (dále jen baseline)
	- Ground Truth Dataset
		- Ablační test: postupně zapínají komponenty a měří % zachycených z 1 649 domén
		- Jen LLM
			- Domény jdou do LLM jen s promptem, bez DNX a TRV
		- LLM + DNX
			- Každá doména má přiřazenou nejbližší legit doménu, páry seřazené a v chunkách
		- LLM + DNX + TRV
			- Navíc must-pass domény, 4 validace a opakování chunků
		- Výsledky (celkem): GPT-3.5 49,5 → 84,2 → 89,9 %, GPT-4o 93,1 → 96,1 → 96,2 %, Llama-3-70B 86,8 → 92,7 → 94,7 %
			- Největší skok přináší DNX
			- Nejvíc pomáhá slabšímu modelu (GPT-3.5 TLD 6,4 → 88,5 %)
			- TRV u GPT-4o skoro nic nepřidá, u některých typů mírně uškodí a zvyšuje čas
		- Měří jen záchyt (recall), FP z tohoto testu nevyčteš
	![[Pasted image 20261019131542.png]]![[Pasted image 20261019131549.png]]

	- Real-time dataset
		![[Pasted image 20261019131820.png]]
		- LLama-3-70B s DNX i TRV
		- Použití jen nových FQDNs eliminuje možnost false positives
			- Jen FP od dlouho existujících legit domén, ne všechny FP
		- Detekce: 34 359 squatting domén z 2 099 184 FQDN, baseline 13 989 (2,45×)
		- Ruční ověření všech 34 359 detekcí po více než měsíci
			- Navštívili domény a hledali vyhledávačem legitimní služby
			- Žádné jasné známky legitimní veřejné služby nenašli
			- Metodika není popsána a neodpovídá to o tom, kolik domén DomainLynx přehlédl, protože dataset nemá labely
		- VirusTotal (89 enginů, opakované skeny)
			- DomainLynx: 9 557 z 34 359 (27,8 %) označeno aspoň 1 enginem
			- Baseline: 3 725 z 13 989 (26,6 %)
			- Podobný podíl -> autoři tvrdí, že víc detekcí nesnižuje přesnost
			- Detekce neoznačené VirusTotalem prezentují jako nové/skryté hrozby
		- Přesnost tím není dokázaná, neoznačeno VirusTotalem neznamená škodlivé
		- Nejvíce se objevuje ComboSquatting
- Závěr
	- Shrnutí výsledků
		- Ground Truth: 94,7 % záchyt s Llama-3-70B + DNX + TRV
		- Real-time: 34 359 detekcí z 2 099 184 nových FQDN, 2,45× víc než baseline
		- 85,65 % detekcí cílí na domény mimo Tranco top 1 000
		- Autoři vidí LLM přístup jako vhodný pro odhalení skrytých a nově vznikajících hrozeb
	- Limitace
		- Závislost na LLM
			- Výkon silně závisí na kvalitě použitého LLM
			- Aktualizace modelu může změnit citlivost detekce, systém je třeba periodicky ladit
		- False positives
			- Úplně odstranit FP je podle autorů stále obtížné
			- Některé detekce nejsou označené VirusTotalem ani při ruční kontrole nevykazují jasný škodlivý záměr
			- Příklady FP: regionální varianty webů značky, záměrně podobná jména domén používaná legitimně
			- Potřeba dlouhodobého vyhodnocení v reálném provozu
		- Evolving threats
			- Útočníci mohou vytvářet domény, které zneužívají slabiny LLM (např. jemné významové posuny)
			- Systém proto potřebuje průběžné úpravy
			- Jen hypotéza, ve studii bez příkladu i testu
			- Claude žádné studie nenašel
				- Výsledky researche: [[Rešerše Evolving threats (Claude)]]
