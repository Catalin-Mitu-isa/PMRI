# Lab 1. Analiza unui fisier malitios

Scopul intregii lucrari de laborator e sa identificati comportamentul si utilitatea unui program malitios. De ce a fost el creat, ce scop se urmareste cu el.

## Analiza statica
Analiza statica consta in analizarea unei entitati malitioase fara rularea ei.

## Analiza dinamica
Analiza efectuata prin rularea entitatii malitioase.

## Intrebarile la care trebuie de raspuns

#### Sub ce format exista aceasta entitate malitioasa?

E sub forma de simplu binar, fisier office cu script, fisier cu script simplu interpretat de powershell sau cmd, "shortcut" care apeleaza cmd/powershell cu un script ca argument, etc?

#### Ce urmareste?

Se face hex dump sau pur simplu analiza stringurilor human readable.
Sa intelegi ce apeluri catre sistemul de operare face? Poti prezice comportamentul lui dupa apelurile de sistem care le utilizeaza? Utilizeaza ceva apeluri mai specifice, mai rar intalnite?

#### Din ce categorie de malware face parte?

E droper, e ransomware, e destoroyer, e cryptominer, e bot?
De unde stii toate astea, de unde ai inteles ca el face una din astea. Dupa ce comportament?

#### Urmareste persistenta in sistem?

Se pune in autostart sau nu? Prin ce metode?

#### Ce alte actiuni ai observat?

Ai urmarit comportamentul lui si ai vazut ce a facut. A creat ceva fisiere? A scris in registri? A deschis vreun port? A incrcat sa faca ceva cu drepturi elevate?

## Instrumente

Instrumente pentu analiza:
- [PEStudio](https://www.winitor.com/download2);
- [ProcessMonitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon);
- [ProcDot](https://procdot.com/downloadprocdotbinaries.htm);
- [Process Hacker](https://processhacker.sourceforge.io/downloads.php);
- [Wireshark](https://www.wireshark.org/);
- [Autoruns](https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns);
- [Ghidra](https://ghidra-sre.org/);
- [x64dbg](https://x64dbg.com/);
- [Cuckoo Sandbox](http://docs.cuckoosandbox.org/en/latest/installation/).

## Pasi

### 1. Setarea mediului de testare

1. Descarcati si instalati [virtualbox](https://www.virtualbox.org/);
2. Setati masina virtuala. Sunt doua moduri de a face acest lucru:
    - Descarcati o [masina virtuala](https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/) deja creata de cei de la microsoft. Ea nu necesita instalare ca un iso, doar descarcati, importati si rulati.
    - Instalarea manuala dupa o imagine. Descarcati iso de pe [pagina oficiala](https://www.microsoft.com/software-download/windows11) microsoft (Va dau aici un siretlic. Nu stiu daca s-a rezolvat de microsoft deja, dar merita incercat. La instalare setati limba engleza world, astfel nu se va instala bloatware) sau puteti lua o versiune windows foarte taiata, [GhostSpectre](https://tech-latest.com/ghost-spectre-windows-11/). Consuma undeva 0.8GB ram la boot. Aceste iso sunt facute de persoane third-party, nu de microsoft, aveti grija cu ea. Folositi doar pentru teste.
    
    Invatati ce sunt snapshot-urile si de ce avem nevoide de ele. [Aici](https://www.howtogeek.com/150258/how-to-save-time-by-using-snapshots-in-virtualbox/) aveti un mic si clar articol legat de acest subiect.
    
    Ar trebui sa aveti snapshot:
    - dupa ce ati setat masina virtuala pentru prima data;
    - dupa ce ati instalat toate aplicatiile de care aveti nevoie pentru analiza;
    - inainte de orice rulare a entitatii malitioase. Acest lucru va permite sa rulati malware de cate ori vreti si de fiecare data pe curat, ca si cum ar fi rulat pentru prima data pe acea masina.

3. Instalati-va aplicatiile necesare pentru analiza (cititi pana la sfarsit pentru a sti exact de ce veti avea nevoie).
4. Mutati fisierul malitios pe masina virtuala.
5. Inchideti toate [fisierele comune](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fi.ytimg.com%2Fvi%2F9-teQnZ8LEY%2Fmaxresdefault.jpg&f=1&nofb=1&ipt=5e92e63d84e0ebbefcc30a916ce628e443edfdee594b3c0cc2a4053f092923c2&ipo=images) si [adaptoarele de retea](https://www.codesandnotes.be/wp-content/uploads/2018/10/VirtualBox-virtual-machine-adapter2-settings.jpg) pentru acea masina. Pentru inceput e recomandat de avut acea masina total izolata. E clar ca ar putea limita comportamentul unei entitati malitioase, in special pentru dropperi. In caz ca aveti un dropper de analizat sau o alta entitate a carui comportament malitios depinde de ce se descarca online, atunci alegeti altceva.

Nota la 5: In caz doriti sa permiteti acces la internet pentru acel malware, ati putea seta o a doua masina virtuala (GNU) care va functiona ca proxy. Le vom numi masina 1 (proxy) si masina 2 (unde se ruleaza malware). In setarile de retea din virtualbox a masinilor virtuale setati retea ce permite comunicarea doar intre ele si cu nimeni altul. Acest tip de retea se numeste [internal](https://www.virtualbox.org/manual/ch06.html).
Masina nr 1 va avea, pe langa acest adaptor de retea interna, inca un adaptor de tip NAT, pentru acces extern. Pe masina 1 configurati [proxy server](http://www.squid-cache.org/) si tor. In setarile masinii 2 puneti setarile de proxy sa treaca prin proxy setat pe masina 1.

### 2. Analiza statica
Acesti pasii sunt minimul pentru o analiza statica. Puteti sa extindeti analiza dupa cum doriti.

1. In caz ca e fisier binar, se analizeaza continutul la nivel de biti. Unele fisiere malitioase contin o foarte mica parte din fisier pentru insasi comportament si restul le umple cu zero-uri sau cu valori aleatorii. Marimea lor mare permite sa evite detectia de unele scanere (Spatiu Cloud, VirusTotal, etc.) Puteti utiliza orice hex viewer sau hex editor pentru aceasta sarcina. Windows are preinstalat `format-hex`. Pentru a vizualiza in consola: `format-hex fisierul-malitios | more`, pentru a salva in fisier: `format-hex fisierul-malitios > numele-fisierului-de-iesire`.
2. Utilizati rezultatele de mai sus sau puteti utiliza PEStudio pentru scanarea textelor posibil citite de om. In ele se pot gasi nume de domen, link-uri, etc.
3. Utilizati ILSpy sau Ghidra pentru reverse engineering si analiza codului/pseudo codului in baza fisierului.

### 3. Analiza dinamica (cel mai interesant si infricosator)

1. Inainte de a rula fisierul malitios, nu uitati de a crea un snapshot la masina virtuala, pentru a putea reveni si face testul din nou, la necesitate.
O idee buna ar fi inregistrarea video masinii virtuale. Virtual box ofera deja aceasta posibilitate, fara a fi nevoie de aplicatii suplimentare.
2. Rulati wireshark si procmon pentru inregistrarea activitatii in sistem si in retea. Puteti utiliza ProcessHacker pentru a observa mai comod decat TaskManager ce procese creaza malware, iar la necesitate sa-l omorati.
3. Salvati rezultatele de la process monitor in format csv (comma separated values) si rezultatele din wireshark in format pcap.
4. Utilizati ProcDot pentru a prelucra rezultatele obtinute in pasul 3. Formatul grafic al acestor date vor usura cu mult procesul de analiza a comportamentului entitatii malitioase.