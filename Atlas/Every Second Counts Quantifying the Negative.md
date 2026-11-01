**Tagy:** #
**Link:** [[Every Second Counts Quantifying the Negative.pdf]]

---

## Shrnutí

- Data: pasivní HTTP/DNS logy z univerzity (TAP) a firemní proxy (PROXY), uživatelé anonymizovaní
	- pocity uživatelů se neměří (žádný dotazník), sleduje se jen čas a zda uživatel cíl našel
	- předpoklad: každá ztracená sekunda má stejnou hodnotu (i když sekunda před deadlinem je cennější)
- Co uživatel udělá po překlepu
	- přepíše adresu v řádku → nejčastější (po typosquat doméně 38 403 návštěv, po neregistrované 54 760)
	- vyhledá správnou adresu ve vyhledávači (1 797 vs. 11 604)
	- klikne na odkaz na stránce (243 vs. 1 088)
	- u defenzivní domény ho stránka většinou rovnou přesměruje (38 712)
- Jak dlouho to trvá
	- defenzivní doména: ~2,87 s, vzdá to 3,3 % lidí
	- typosquat doména: ~9,58 s, vzdá to 16,81 %
	- neregistrovaná doména (chybová stránka): ~10,38 s, vzdá to 11,53 %
	- uživatel se zorientuje skoro stejně rychle u reklamní stránky jako u chybové hlášky
	- reklamní stránka sama dává signál „tady nejsem správně“, horší je stránka podobná originálu nebo konkurence → delší hledání nebo vzdání se
- Rychlé opravy
	- 30 % uživatelů opraví překlep do 5 s
	- přes 40 % oprav přes vyhledávač je pod 5 s → adresní řádek funguje i jako vyhledávání a vyhledávač překlep sám opraví
	- část „překlepů“ jsou artefakty prohlížeče: předběžné DNS dotazy během psaní (example.co při psaní example.com)
- Mobil vs. počítač
	- na počítači jsou uživatelé měřitelně rychlejší, při ručním přepsání adresy jen nepatrně
- Na co uživatelé dělají překlepy
	- hlavně top 100 000 webů, ale 15 % míří na weby mimo top 1 M
	- nejvíc překlepů u domén dlouhých ~12 znaků
	- časté jsou i „logické“ chyby: ratemyprofessor.com místo ratemyprofessors.com, hello-world vs. helloworld (nepochopení, ne překlep)
	- uživatelé často napíšou do řádku název vyhledávače a teprve tam hledají
- Celková ztráta času
	- všechny překlepy na univerzitě dohromady: 1 205 060 s (necelé 2 týdny)
	- vina typosquatterů: max. ~4,22 s na 24 h prohlížení, v USA ~64 s ročně na člověka (~0,29 $)
	- v průměru typosquatteři uživatele spíš nezdrží, někteří ho k cíli dostanou rychleji než chybová stránka
	- weby přijdou o ~5 % lidí, kteří se překlepnou
