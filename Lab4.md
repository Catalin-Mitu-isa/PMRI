# Lab 4. Instrument de analiză criminalistică digitală automatizată

**Scopul lucrarii:** implementarea unei functii de analiza criminalistică digitala automata.

## Sarcini

De efectuat un program (CLI sau GUI, nu are importanta) utilizand WinAPI, nu biblioteci, ce va contine unul din urmatoarele functii:

_(Se alege in baza listei grupei. Pentru a concretiza varianta, doar intrebati-ma)_

**1.** Se va extrage infomatia tuturor proceselor ce ruleaza in acel moment pe calculator si se va salva in format json intr-un fisier urmatoarele:
- ID-ul procesului;
- Adresa fisierului executabil;
- Starea semnaturii digitale, daca e de incredere sau nu.

**2.** Se va salva in format json intr-un fisier urmatoarele:
- ID-ul procesului;
- Adresa si portul local;
- Adresa si portul destinatie, daca e prezent;
- Protocolul utilizat;
- Starea.
<br/>Daca un proces nu are nici un socket deschis, nici nu va fi prezent in acel fisier.

<!-- **3.** Se vor afisa toate discurile conectate la sistem si, dupa selectarea de utilizator a unui disc, se va crea o imagine in baza acelui disc, care va fi utilizat ulterior pentru analiza.
[Volume Shadow Copy Service](https://learn.microsoft.com/en-us/windows-server/storage/file-server/volume-shadow-copy-service) -->

**Conditii:**
- De utilizat WinAPI direct, fara biblioteci;
- Codul va fi pus in repozitoriu public;
- Codul va fi insotit de comentarii si link-uri catre documentatie;
- In rezultatele sarcinilor trebuie sa fie prezent si programul malitios dezvoltat la [lucrarea de laborator nr. 2](Lab2.md).

