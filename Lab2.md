# Lab 2. Crearea unui program malitios

**Scopul lucrarii:** crearea unui program malitios.

## Conditii

Se vor utiliza limbajele de programare compilabile: C, C++, Rust, C# (doar windows), Go, D.
In ajutor pot veni si limbaje de scripting: PowerShell, Bash, Batch.

Programul malitios ar trebui sa cuprinda urmatoarele:
- [obfuscare](#obfuscarea);
- [persistenta in sistem](#persistenta-in-sistem);
- [utilizarea rationala a resurselor](#utilizarea-rationala-a-resurselor);
- [detectarea rularii intr-un mediu controlat](#detectarea-rularii-intr-un-mediu-controlat);
- [comunicare cu centrul de control](#comunicare-cu-centrul-de-control);
- [metode de evitare antivirus](#metode-de-evitare-antivirus);

**Codul va fi pus intr-un repo public pe github. Link-ul va fi pus in raport.**

## **Pasii**

**1\. Definiti scopul vostru. Ce doriti sa obtineti cu acest program malitios?**

Cele mai populare scopuri in randul dezvoltatorilor de programe malitioase:
- Câștig financiar: furt de informații financiare, ransomware, cryptoware.
- Spionaj și supraveghere, colectare de informații sensibile.
- Hacktivism: Crearea de malware ca formă de protest sau activism;
- Război cibernetic;
- Furt de proprietate intelectuală;
- Recrutare botnet.

**2\. Proiectarea**

Ca orice proiect, trebuie de inceput cu modelarea lui, definirea comportamentului si functionalitatilor.
Creati o lista de sarcini cu prioritatea lor. Dezvoltati initial functionalul de baza, apoi, daca doriti sau reusiti, extindeti-l.

**3\. Setati-va mediul de dezvoltare**

Utilizati masina virtuala ce ati folosit-o la analiza malware (lab. 1), doar restabiliti starea masinii la una curata.

**4\. Insasi dezvoltarea si testarea**

Informatii suplimentare:

<span id="obfuscarea">**Obfuscarea.**</span>

- C++ [String](https://github.com/adamyaxley/Obfuscate) and [code](https://github.com/fritzone/obfy) obfuscator;
- [Rust obfuscator](https://docs.rs/goldberg/latest/goldberg/);
- [C# Obfuscator](https://github.com/obfuscar/obfuscar).

<span id="persistenta-in-sistem">**Persistenta in sistem.**</span>

- [Informatii](https://github.com/Karneades/malware-persistence?tab=readme-ov-file) legate de persistenta in sistem de catre programe malitioase.

<span id="utilizarea-rationala-a-resurselor">**Utilizarea rationala a resurselor**</span>

In caz da avem un bot utilizat la DDoS-uri, avem un stealer si transmitem multa informatie catre centrul de control, descarcam informatie de pe serverele malitioase, trebuie sa avem grija ca programul nostru sa nu se evidentieze printre altele. Sa nu fie el motivul ca utilizatorul simte ca calculatorul se misca mai greu decat de obicei.

Putem reduce consumul de memorie ram printr-un cod mai optimizat si gandit sa utilizeze mai rational resursele. De exemplu, in caz ca vrem sa transmitem un fisier de 200MB in extern, nu-l incarcam 100% in memorie, iar apoi il transmitem. O facem pe bucati mici. Citim o parte, transmitem, citim alta bucata, transmitem.

Pentru a reduce utilizarea procesorului putem limita executia doar la un nucleu, limita prioritatea firelor de executie, limita numarul de fire, face pauze neordonate (timp diferit de fiecare data).

<span id="detectarea-rularii-intr-un-mediu-controlat">**Detectarea rularii intr-un mediu controlat**</span>

Aici sunt doua verificari, una pentru malware final, alta pentru versiunea de testare. Implementati in cod o metoda de a verifica daca rulati malware pe calculatorul gazda sau in mediul controlat. Puteti pune orice doriti, in versiunea finala nicicum nu ar trebui sa ajunga. Daca se descopera ca la prezenta fisierului cu numele, de exemplu, "no-run" malware nu mai ruleaza, atunci sunteti opriti complet.
Puteti cauta un fisier cu un nume anumit, seta o variabila globala in sistem, verifica numele sistemului (hostname), cum doriti.

La subiectul de detectare VM sunt numeroase metode. Doar deschideti DeviceManager pe masina virtuala si numarati daca macar odata ati intalnit numele VirtualBox.
Alte indicii ar putea fi lipsa interactiunii din partea utilizatorului, prea putina memorie alocata, la fel ca si numarul de nuclee.

Puteti face singuri detectarea de VM sau utiliza [biblioteci](https://github.com/search?q=vm%20detection&type=repositories) (in acest caz ar trebui sa stiti ce se intampla acolo si cum se face verificare, nu doar utilizati cu ochii inchisi).

<span id="">**Comunicare cu centrul de control**</span>

Pentru a mentine o cumunicare de lunga durata cu centrul de comanda e nevoie de mai multa munca si tehnici de evaziune.
Astfel, pentru aceasta lucrare de laborator, veti seta o simpla comunicare intre malware si un centru de comanda.
Malware va face conexiunea cu serverul la intervale de timp variabile catre server pentru a primi sau transmite informatii, depinde de necesitatile voastre.
Se poate de facut un server banal care raspunde la metodele `GET` sau `POST`.
De exemplu, adresa serverului este `192.168.0.13`, iar
portul la care asculta e `6666`:
- Se face o cerere `GET` la fisierul `commands` si serverul raspunde cu ce comenzi are la moment.
- Se face o cerere `POST` catre fisierul `collected-data` impreuna cu informatia care a colectat-o de pe masina infectata.

<span id="metode-de-evitare-antivirus">**Metode de evitare antivirus**</span>

1. Implementati o pornire intarziata. Rulati partea de baza a fisierului malitios doar dupa un timp anumit, 2 ore, 3 zile, cum vi-i comod.
2. Creati o versiune noua a fisierului la fiecare compilare. Dupa cum stiti, chiar si un bit modificat, va modifica complet si valoarea hash a programului. Pentru a avea un hash diferit de fiecare data, e nevoie de a modifica un singur bit. Cea mai simpla metoda e de a utiliza meta programare pentru a genera o valoare noua ce va fi salvata intr-o variabila statica globala.