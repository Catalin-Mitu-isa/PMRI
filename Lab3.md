# Lab 3. Analiza unui calculator infectat de malware.

**Scopul lucrarii:** analiza actiunilor unei entitati malitioase pe un calculator deja infectat.

La lucrarea de laborator nr. 1 a fost efectuata analiza in timp real. S-a avut entitatea malitioasa la indemana, s-a putut analiza static, s-a putut pregati masina pentru a analiza in timp real activitatea sa. De data asta avem doar rezultatele actiunii sale si informatia care sistemul de operare a salvat-o.

    Gasiti o alta entitate malitioasa decat cea utilizata in lab 1.
    NU malware de trolling.

    Найдите другую вредоносную сущность, не то, которая использовалась в лабораторной работе 1.
    НЕ для троллинга.

    Find another malicious entity. Other than the one used in lab 1.
    NO trolling malware.

Rulati si asteptati pana se face liniste sau pana considerati ca a trecut suficient timp ca masina sa fie infectata.

## Extragerea sigura a informatiei despre masina infectata

#### Memory dump

Aflati uuid a masinii ce analizati:

```
VBoxManage list vms
```

Extrageti dump cu urmatoarea comanda:

```
VBoxManage debugvm <uuid> dumpvmcore --filename=<numelefisierului>.elf
```

#### Disk dump

VirtualBox ofera aceasta posibilitate, dar doar pentru un disk, adica e nevoie de unit snapshot-urile si discul de baza in unul singur.

Salvati starea masinii si vi se va face disponibil butonul de clonare a masinii virtuale.

Cum in [imagine](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fdocumentation.help%2FVirtualBox%2Fclone-vm.png&f=1&nofb=1&ipt=a6ae0bf9f7f99834660a0b8b8e55d97d5250a8928dd8ef61158255a205d94462&ipo=images), e nevoie de selectat Clone type: Full clone, Snapshots: current machine state.

Comanda urmatoare va va afisa uuid si locatia noului disc:

```
VBoxManage list hdds
```

Cu ajutorul comenzii urmatoare veti converti din formatul propriu al virtualbox, vdi, in format raw.

```
VBoxManage clonemedium <uuid> <nume-fisier>.img --format=raw
```

[Aici](https://www.virtualbox.org/manual/ch08.html) gasiti documentatia pentru comenzile de mai sus.

## Analiza fisierelor extrase

### Analiza memoriei

Se va folosi instrumentul [volatility3](https://github.com/volatilityfoundation/volatility3). Urmariti [acesti](https://github.com/volatilityfoundation/volatility3?tab=readme-ov-file#quick-start) pasi pentru setare si rulare.<br/>

[Aici](https://volatility3.readthedocs.io/en/latest/volatility3.plugins.windows.html) puteti gasi documentatie pentru module in v3. [Aici](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference) pentru v2. Unele au fost redenumite, insa inca contin informatie relevanta si pentru v3.

Ce se poate afla dintr-un memdump:

1. Procesele ceare ruleaza;
2. Ierarhia proceselor;
3. Locatia de pornire;
4. Conexiuni active;
5. Handle deschise.

Module utile: psscan, pslist, pstree, cmdline, malfind, netscan, handles.

### Analiza discului

Utilizati [Arsenal Image Mounter](https://arsenalrecon.com/downloads) pentru a conecta fisierul iso obtinut in sistemul de fisiere al calculatorului unde va avea loc analiza.

Metode de a afla mai multe informatii despre executia fisierului malitios:

<!-- Am incercat OSFMounter, acolo dadea problema ca nu puteam accesa nici un fisier din dosar Windows.
Am incercar FTK Imager. Acolo tot imi dadea "unallocated space" si nu am putut deschide nimic.
Am incercat Arsenal Image Mounter, acolo am putut accesa dosarele din Windows. -->

- Verificarea dosarelor cheie gasite in urma analizei memoriei. Unele urme mai pot fi detectate.
- Daca entitatea malitioasa s-a pus in [autostart](https://www.winhelponline.com/blog/analyze-offline-system-autoruns-feature/), putem afla locatia sa curenta unde se ascunde.
- Analiza fisierelor prefetch (X:\\Windows\\Prefetch) ne pot indica orele rularii malware-ului, dosarele si fiserele ce le-a atins (Doar primele 10s a rularii). Instrumente utile:  [PFCmd.exe](https://www.sans.org/tools/pecmd/) si [WinPrefetchView](https://www.nirsoft.net/utils/win_prefetch_view.html).
- Analiza fisierelor [_HIVE_](https://learn.microsoft.com/en-us/windows/win32/sysinfo/registry-hives). Se pot incarca in [regedit](https://forsenergy.com/en-us/regedit32/html/b3a394dd-0ee9-496b-bf16-cc90bf18289d.htm) sau e posibil de utilizat instrumentul [RegRipper](https://github.com/keydet89/RegRipper3.0).
- Analiza jurnalelor de evenimente. Ele sunt localizate in X:\Windows\System32\winevt\Logs\. [FullEventLogView](http://www.nirsoft.net/utils/full_event_log_view.html)