**Tagy:** #WHOIS
**Link:** [[From WHOIS to WHOWAS.pdf]]

---

## Shrnutí

![[Pasted image 20261019162411.png]]
- Data z roku 2019
- Analyzuje 1.2 miliardy záznamů
- Protokol
	- TCP request na port 43
- ICANN vytvoří TLD a deleguje je registrům, které provozují zóny
- Registry pak prodeje koncovým zákazníkům přenechávají registrátorům, od nichž si doménová jména pořizují registranti
- Registrátoři drží kontaktní data registrantů
- Thick WHOIS model předává registrátor kopii těchto údajů registru, ve thin modelu jej drží jen registrátor
	- .com, .net a .jobs používají thin, přechod na thick měl být do 2020
	- Odložen kvůli GDPR
	- Dnes to funguje tak, že se data předají, pokud k tomu existuje právní důvod
- Co se začerňuje (redacted) podle ICANN Temporary Specification (17. 5. 2018, Tabulka I)
	- Jméno, ulice, město, PSČ, telefon, fax: u držitele, administrativního, technického a dalšího kontaktu
	- Organizace, stát/provincie, země: jen u administrativního, technického a dalšího kontaktu (u držitele se **nezačerňují**)
	- E-mail: u držitele, administrativního a technického kontaktu
	- Místo údaje se uvede hodnota „podobná *redacted for privacy*“, nebo se pole nezveřejní vůbec
	- Registrátor musí místo e-mailu nabídnout anonymizovanou adresu nebo webový formulář pro kontakt s držitelem
	- Bez souhlasu držitele se začerňuje vždy, mlčení = nesouhlas
	- Fakturační kontakt specifikace neřeší, ve WHOIS se zveřejňuje jen zřídka
	- Nezačerněné zůstávají údaje bez osobních dat: datum registrace/expirace/aktualizace, registrátor (IANA ID), nameservery
	![[Pasted image 20261019162435.png]]

- Začerňování se týká skoro všech, nejen EU
	- Dle studie přes 85% velkých poskytovatelů začerňuje údaje i mimo EU
- Začernění nemá jednotný zápis
	- redacted for privacy
	- non-public data
	- atd...
- Země držitele se většinou nezačerňuje
	- Dnes to může být jinak
- Hodně studií před 2019 stavělo na datech, so jsou dnes začerněné