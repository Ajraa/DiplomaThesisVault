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
		- 1649 domén cílících na Tranco top 1k, které byly flagnuty aspoň 4 tooly