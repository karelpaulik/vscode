# Přihlášení přes VSCODE na Linuxový server

VSCODE pracuje s formátem klíče: .pem. Tento klíč se přímo stáhne z AWS.

Pokud k dispozici .ppk, pak možno přes "puttygen" převést na .pem.

# puttygen:
1. Load existing private key file, načíst .ppk soubor
2. Conversions - Export openSSH key

### Nastavení práv na souborech
Na těchto souborech musí být malý počet majitelů práv.
Je nutno mít správně nastavena práva na souborech:
- xxx.pem  
- C:\Users\karel.paulik\.ssh\config  Pozn. Tento soubor se vytváří, při otevírání spojení ve VSCODE.
  
Jak na to:

- Vybrat soubor
- RMB - Vlastnosti
- Zabezpečení
- Upřesnit - zakázat dědičnost - Převést zděděná oprávnění na ... (Jinak nebue fungovat další krok)
- Upravit - Ponechat pouze jednoho uživatele - mě. Plný přístup asi není potřeba: ČÍST + ČJÍST A SPOUŠTĚT.

### VSCODE
Pro vzdálené připojení přes VSCODE je potřeba mít nainstalovány extension:
- Remote - SSH
- Remote - SSH: Editing Configuration Files

Na levé straně VSCODE (pod: Explorerem, Vyhledáváním, Source Control, ...) se nově oběví ikona: Remote Explorer. Zde dát: Nastavení (ikona ozubeného kola) - Select SSH configuration file to update: Vybrat: C:\Users\karel.paulik\.ssh\config

Nastavení configu:
Pozn. Umístění tohoto souboru je: C:\Users\karel.paulik\.ssh\config
Není potřeba vytvářet/editovat tento soubor, tento soubor vyskočí automaticky při konfiguraci vzdáleného připojení přes VSCODE.

```
Host Linux-A
    HostName 3.126.248.125
    User ubuntu
    IdentityFile "D:\Data\AWS\2\LightsailDefaultKey-eu-central-1.pem"
```

- Uložit soubor.
- Pozor: Musí být IP adresa. S doménou to nefunguje.

Vybrat dle zadaného názvu v configu: "Linux-A" - Connect

### Odpojení: 
- F1
- Remote-SSH: Kill VS Code Server on Host...

### Kontrola spojení přes "Powershell":
- Spustit powershell (není třeba as admin)
- ssh -i D:\Data\AWS\2\LightsailDefaultKey-eu-central-1.pem ubuntu@3.126.248.125
- musí být zadaná IP adresa. bedobe.eu to nebere

### Jak odstranit připojení, které je definované v VSCODE
Ve VSCODE nejde přes UI vybrat existující připojení. Je nutno otevřít config daného připojení a smazat jeho obsah.
  
