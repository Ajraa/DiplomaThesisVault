# Rešerše: obcházení (LLM) detektorů squatting/phishing domén – limitace „Evolving threats"

Kontext: DomainLynx (Chiba, Nakano, Koide, CCNC/IEEE Access 2025) v limitacích jen hypoteticky zmiňuje domény, které „exploit specific weaknesses in LLM comprehension, such as using subtle semantic shifts". Cíl: najít studie, které tento problém zkoumají nebo měří.
Datum rešerše: 2026-10-19. Čísla jsou převzata z textů studií (arXiv HTML / stránky konferencí); u položek s ⚠ je potřeba ověřit v PDF.

Relevance: **přímo** = měří obcházení LLM detektoru nebo squatting detektoru; **částečně** = příbuzný útok nebo detektor; **okrajově** = analogie / kontext.

---

## A. Peer-reviewed

### 1. Koide, T., Fukushi, N., Nakano, H., Chiba, D. (2023). *PhishReplicant: A Language Model-based Approach to Detect Generated Squatting Domain Names.* ACSAC 2023, ACM. DOI 10.1145/3627106.3627111
Odkaz: https://arxiv.org/abs/2310.11763 · https://www.acsac.org/2023/program/final/s48.html
- **Co zkoumá:** „Generated squatting domains" (GSD), tedy domény, které útočníci sériově vytvářejí kombinací více technik (typo + combo + klamavé subdomény), často bez přesného názvu značky. Navrhuje detekci přes podobnost s již známými phishingovými doménami (Sentence-BERT + shlukování), ne přes seznam značek.
- **Hlavní čísla:** Za 4 týdny v reálném provozu 3 784 detekcí, z toho 3 498 (92,4 %) potvrzeno. Srovnání na datech z března 2023: shoda s threat intelligence / Google Safe Browsing 26,1 % u PhishReplicant vs. **0,4–1,9 % u dnstwist, Phishing Catcher, StreamingPhish a Ctl-pipeline**. Domén bez přesného názvu značky: 1 800 vs. 480 (dnstwist). Citace: *„many domain names are created by using multiple squatting techniques to evade existing detection methods."*
- **Relevance:** **přímo** pro bod 4 (pravidlové nástroje jako dnstwist míjí kombinované a bezznačkové domény) a bod 3 (útočníci domény generují systematicky). Pozn.: samotná metoda používá dotrénovaný jazykový model, do práce tedy jen jako zdroj zjištění, ne jako postup.

### 2. Kulkarni, A., Balachandran, V., Divakaran, D. M., Das, T. (2025). *From ML to LLM: Evaluating the Robustness of Phishing Web Page Detection Models against Adversarial Attacks.* Časopis ACM, DOI 10.1145/3737295 (⚠ název časopisu ověřit, pravděpodobně ACM Digital Threats: Research and Practice).
Odkaz: https://dl.acm.org/doi/10.1145/3737295 · https://arxiv.org/abs/2407.20361
- **Co zkoumá:** Nástroj PhishOracle vkládá do phishingových stránek obsahové a vizuální změny (průhlednost loga, rotace, šum, skryté odkazy) a generuje i podobné domény (homoglyf + přípona typu „-login"). Testuje ML detektor, VisualPhishNet, Phishpedia a LLM detektor (Gemini 1.5 Flash).
- **Hlavní čísla:** Recall Phishpedia klesl z 81,6 % na 40 %, stack modelu z 98,3 % na 70,9 %. LLM detektor (screenshot) F1 98,0 % → 88,6 %, recall 79,5 %. V jednom případě LLM považoval doménu podobnou Yahoo za doménu stejné firmy. Autoři: LLM detektor je odolnější, ale *„still is prone to evasion."*
- **Relevance:** **částečně**. Jediná recenzovaná studie, která měří pokles LLM phishing detektoru pod útokem, ale hlavním vektorem je obsah stránky, ne doménové jméno.

### 3. Hu, H., Jan, S. T. K., Wang, Y., Wang, G. (2021). *Assessing Browser-level Defense against IDN-based Phishing.* USENIX Security 2021, s. 3739–3756.
Odkaz: https://www.usenix.org/conference/usenixsecurity21/presentation/hu-hang
- **Co zkoumá:** Pravidlové politiky 7 prohlížečů (5 desktop, 2 mobilní; verze 2015–2020), které rozhodují, zda IDN homograf zobrazit v Unicode nebo Punycode. Černoskříňové testování přes 9 000 testovacími případy plus uživatelská studie.
- **Hlavní zjištění:** Všechny testované prohlížeče mají slabiny v pravidlech, které umožní vytvořit homograf, jenž obranu obejde. Pravidla se časem nezpřísňují (Chrome jedno pravidlo vrátil zpět). Homografy, které obranu obešly, uživatele stále klamou. (⚠ přesné počty obcházejících domén jsou jen v PDF.)
- **Relevance:** **přímo** pro bod 4. Dobrý doklad, že i čistě pravidlová (heuristická) ochrana je obchoditelná, jakmile útočník pravidla zná.

### 4. Yuan, Y., Apruzzese, G., Conti, M. (2022). *SpacePhish: The Evasion-space of Adversarial Attacks against Phishing Website Detectors using Machine Learning.* ACSAC 2022, ACM. DOI 10.1145/3564625.3567980 (rozšíření „Multi-SpacePhish" v časopise ACM).
Odkaz: https://arxiv.org/abs/2210.13660
- **Co zkoumá:** Realistické levné úniky (bez přístupu k detektoru) proti 18 ML detektorům phishingových webů, 12 útoků. Nejlevnější útok nahradí URL zkrácenou URL (bit.ly), další mění HTML.
- **Hlavní čísla:** Realistické úniky snižují detekci o 3–10 % (statisticky významně). Kombinace změn webu a příznaků sníží detekci z 0,95 na 0.
- **Relevance:** **částečně**. Ukazuje, že i jednoduchá manipulace s URL mění lexikální příznaky. Detektory jsou ML, ale lexikální znaky URL jsou stejné jako v heuristickém skóre.

### 5. Kim, T., Park, N., Hong, J., Kim, S.-W. (2022). *Phishing URL Detection: A Network-based Approach Robust to Evasion.* ACM CCS 2022.
Odkaz: https://arxiv.org/abs/2209.01454
- **Co zkoumá:** Útočníci napodobují vzory benigních URL (sdílený hosting, benigní domény, IP adresy), aby obešli detektory založené na příznacích URL řetězce. Navrhuje grafovou metodu přes síťové sousedy.
- **Hlavní čísla:** F1 0,89 vs. 0,84 nejlepší příznakové metody. Phishingová URL zůstane detekovatelná, dokud útočník nezmění i většinu jejích síťových sousedů.
- **Relevance:** **okrajově až částečně**. Argument, proč samotné lexikální znaky nestačí a proč přidávat infrastrukturní signály (DNS, hosting, WHOIS).

### 6. Spracklen, J., Wijewickrama, R., Sakib, A. H. M. N., Maiti, A., Viswanath, B., Jadliwala, M. (2025). *We Have a Package for You! A Comprehensive Analysis of Package Hallucinations by Code Generating LLMs.* USENIX Security 2025.
Odkaz: https://www.usenix.org/conference/usenixsecurity25/presentation/spracklen · https://arxiv.org/abs/2406.10279
- **Co zkoumá:** Jak často 16 LLM pro generování kódu vymýšlí neexistující balíčky, které si útočník může zaregistrovat („slopsquatting").
- **Hlavní čísla:** 576 000 vzorků kódu. Halucinované balíčky tvoří nejméně 5,2 % u komerčních a 21,7 % u open-source modelů. 205 474 unikátních halucinovaných názvů.
- **Relevance:** **okrajově**. Jde o balíčky, ne domény, ale je to recenzovaný doklad nového squattingu, který vzniká z výstupu LLM a který klasická typo pravidla nepokrývají. Doménová obdoba je bod 9.

### 7. Agarwal, S., Vasek, M. (2026). *Examining newly registered phishing domains at scale.* Journal of Cybersecurity 12(1), tyag020.
Odkaz: https://academic.oup.com/cybersecurity/article/12/1/tyag020/8735840
- **Co zkoumá:** 15 126 nově registrovaných smishing/phishing domén (07/2023–05/2024): registrátoři, hosting, životnost, detekce.
- **Hlavní čísla:** Google Safe Browsing API zachytilo jen 19,9 % domén. Část domén nepoužívá značku, ale oborová slova (doručování, bankovnictví). Nejzneužívanější registrátor NameSilo nabízí AI nástroj na generování podobně znějících domén.
- **Relevance:** **okrajově**. Kontext k „evolving threats" (AI generátory jmen přímo u registrátora) a k tomu, proč blacklisty nestačí.

---

## B. arXiv a průmyslové zprávy (nerecenzováno)

### 8. Koide, T., Nakano, H., Chiba, D. (2026). *Clouding the Mirror: Stealthy Prompt Injection Attacks Targeting LLM-based Phishing Detection.* arXiv:2602.05484.
Odkaz: https://arxiv.org/abs/2602.05484
- **Co zkoumá:** Stejný tým jako DomainLynx. Nepřímý prompt injection skrytý v HTML nebo **přímo v URL (subdoména, cesta, query, fragment)** proti multimodálním LLM phishing detektorům. Definuje 5 hlavních technik (např. legitimní záminka, převzetí role) a 6 útočných ploch. Navrhuje obranu InjectDefuser (zpevnění promptu, RAG allowlist domén značky, validace výstupu).
- **Hlavní čísla:** Úspěšnost útoku přes URL: GPT-5 20,0 %, Grok 4 Fast 33,5 %, Llama 4 Maverick 22,0 %, Gemma 3 0 %. S obranou GPT-5 0 %, Llama 4 7 %. Přes HTML až 84,7 % (Llama 4). Citace: *„phishing detection with state-of-the-art models such as GPT-5 remains vulnerable to PI."*
- **Relevance:** **přímo** pro LLM detektory a doménu/URL jako útočnou plochu. Nejbližší práce k limitaci DomainLynx, ale měří prompt injection, ne „semantic shifts" v samotném doménovém jméně.

### 9. Nagaraj, K., Marty, D.-O., Kaleli, B., Starov, O. (2026). *Phantom Squatting: AI-Hallucinated Domains as a Software Supply Chain Vector.* Palo Alto Networks Unit 42, 30. 6. 2026.
Odkaz: https://unit42.paloaltonetworks.com/phantom-squatting-hallucinated-web-domains/ (shrnutí CSA: https://labs.cloudsecurityalliance.org/research/csa-research-note-phantom-squatting-ai-hallucinated-domains/)
- **Co zkoumá:** Útočníci se ptají LLM na URL značek a registrují domény, které model vymyslí. 913 značek, 685 339 promptů, dva LLM.
- **Hlavní čísla:** 2,1 mil. URL, 37,3 % vede na neexistující domény (asi 250 000 unikátních „phantom" domén). 13 229 (0,61 %) už označeno jako škodlivé. Mezi halucinací a registrací útočníkem uplynulo 18–51 dní (případ poštovní služby: phishingový kit 23 dní po zachycení). Domény jsou sémanticky věrohodné, typicky značka + funkční slovo (admin, billing, -login, „empresas"), ne překlepy. Citace: *„conventional defensive perimeters have no actionable signal."*
- **Relevance:** **přímo** pro bod 3 (LLM jako zdroj squatting domén) a bod 2 (sémantické a jazykové varianty). Typosquatting nástroje nehodnotí. Nerecenzováno.

### 10. Mashood, F., Nabeel, M. (2026). *Can LLMs Reason About Brand Ownership? An Empirical Study of Domain Attribution Intelligence.* arXiv:2606.20868.
Odkaz: https://arxiv.org/abs/2606.20868
- **Co zkoumá:** Zda 4 LLM (Gemini 2.5/3.5 Flash, Claude Sonnet 4.5/4.6) poznají, že doména patří značce (36 často phishovaných značek). Varianty: jen model, web search, WHOIS, obojí. Negativní vzorky jsou dnstwist varianty.
- **Hlavní čísla:** Bez nástrojů macro-F1 jen 0,18–0,37 (model při nejistotě říká „ne"). S WHOIS 0,77–0,86, se search + WHOIS až 0,87. Search sám nepomohl. Chyba „registrant confusion": s WHOIS Gemini 2.5 Flash přijímal registranta squattera jako značku (přesnost na squattingu 0,124). Modely halucinují věrohodné neexistující domény značek.
- **Relevance:** **částečně**. Neměří adversariální domény, ale ukazuje slabiny LLM v přiřazení domény ke značce a **význam WHOIS** pro tvou práci. Výsledky nejsou rozdělené podle typu squattingu.

---

## Co jsem hledal a nenašel
- Žádnou recenzovanou ani arXiv studii, která by **cíleně generovala doménová jména se sémantickými posuny, synonymy, překlady nebo novými slovy** a měřila, kolik jich projde LLM detektorem squattingu (DomainLynx, PhishReplicant apod.).
- Žádnou studii, která by **měřila úspěšnost domén generovaných útočníkem pomocí LLM proti existujícím squatting detektorům** (Unit 42 měří výskyt a registraci, ne obcházení detektorů).
- Žádnou systematickou studii robustnosti nástrojů typu dnstwist vůči novým technikám kromě srovnání v PhishReplicant (bod 1). Existují jen blogové srovnání (Bolster, Security Boulevard 2022), nepoužitelné jako zdroj.
- Výsledky MDPI Applied Sciences / Scientific Reports / Frontiers 2026 o adversariální robustnosti URL klasifikátorů existují, ale jde o trénované ML modely a menší venue, proto vynechány.

## Závěr
Mezera existuje: pro LLM detektory phishingu je změřen prompt injection (Koide et al. 2026, arXiv) a pro pravidlové nástroje je doloženo míjení kombinovaných a homografních domén (PhishReplicant 2023, Hu et al. 2021), ale obcházení LLM detektorů squattingu pomocí sémanticky posunutých nebo LLM-generovaných doménových jmen zatím nikdo empiricky neměřil.
