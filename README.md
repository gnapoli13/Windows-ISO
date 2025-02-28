
# WINDOWS CUSTOM ISO

Modifica l'ISO di Windows aggiungendo i driver presenti nel sistema in uso prima di effettuare una formattazione


## Procedura

1) Avvia la Powershell come Amministratore

2) Crea le cartelle che ti servono

```bash
mkdir C:\Mount, C:\Drivers
```

3) Estrai i drivers presenti sul sistema

```bash
dism /online /export-driver /destination:C:\Drivers
```

4) Estrai l'ISO di Windows scaricata in C:\Win e verifica le versioni presenti

```bash
dism /Get-WimInfo /WimFile:C:\Win\sources\install.wim
```

5) Modifica l'Index con la versione che ti interessa (Windows 11 Pro ha Index 5)

```bash
dism /Mount-Image /ImageFile:C:\Win\sources\install.wim /Index:5 /MountDir:C:\Mount
```

6) Installa i driver estratti

```bash
dism /Image:C:\Mount /Add-Driver /Driver:C:\Drivers /Recurse
```

7) Smonta l'immagine e committa

```bash
dism /Unmount-Image /MountDir:C:\Mount /Commit 
```

8) Crea la nuova ISO 

```bash
oscdimg -m -o -u2 -udfver102 -bootdata:2#p0,e,bC:\Win\boot\etfsboot.com#pEF,e,bC:\Win\efi\microsoft\boot\efisys.bin C:\Win C:\WinNaple.iso
```

## Attenzione

L'ultimo comando utilizza oscdimg. Bisogna aggiungere al Path delle variabili d'ambiente la cartella che contiene oscdimg per poterlo usare
