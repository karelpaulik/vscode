# Přihlášení přes VSCODE na Linuxový server

VSCODE pracuje s formátem klíče: .pem. Tento klíč se přímo stáhne z AWS.

Pokud k dispozici .ppk, pak možno přes "puttygen" převést na .pem.
### puttygen:
1. Load existing private key file, načíst .ppk soubor
2. Conversions - Export openSSH key

### Nastavení práv na souborech
Na těchto souborech musí být malý počet majitelů práv.
Je nutno mít správně nastavena práva na souborech:
- xxx.pem  
- C:\Users\karel.paulik\.ssh\config  Pozn. Tento soubor se vytváří, při otevírání spojení ve VSCODE.
  
Nastavení práv - Jak na to:

- Vybrat soubor
- RMB - Vlastnosti
- Zabezpečení
- Upřesnit - zakázat dědičnost - Převést zděděná oprávnění na ... (Jinak nebue fungovat další krok)
- Upravit - Ponechat pouze jednoho uživatele - mě. Plný přístup asi není potřeba: ČÍST + ČJÍST A SPOUŠTĚT.

### Kontrola spojení přes "Powershell":
- Spustit powershell (není třeba as admin)
- ssh -i D:\Data\AWS\2\LightsailDefaultKey-eu-central-1.pem ubuntu@3.126.248.125
- musí být zadaná IP adresa. bedobe.eu to nebere

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

### files.watcherExclude - toto není nutné. Je již zahrnuté v bloku níže, kde je to ještě více rozpracováno.
- Settings
- hledat: files.watcherExclude
- přidat: **/node_modules/**
- přidat: **/nodejs/node_modules/**
- uložit
- restartovat VSCODE

### Odpojení: 
- F1
- Remote-SSH: Kill VS Code Server on Host...

### Jak odstranit připojení, které je definované v VSCODE
Ve VSCODE nejde přes UI vybrat existující připojení. Je nutno otevřít config daného připojení a smazat jeho obsah.

# Práce v VSCODE s nodejs
Nepouštět příkazy přes VSCODE terminál. Když jsem dal ve VSCODE "npm install cloudcmd" tak to zablokovalo ubuntu server. Když jsem stejný příklad dal v terminálu, tak příkaz doběhl.

# Nastavení VSCODE na serveru - tak aby pokud možno nepadalo připojení.
Po prvním připojení přes VSCODE je vhodné:
- VSCODE - Manage (ozubené kolo v levo dole) - Settings - hledat: Watcher Exclude
- Zde se dá nastavovat, co nemá VSCODE dělat. Vhodnější je ale cesta přes Linux server, viz níže:

Na Linux serveru vznikne po připojení adresář: 
- .vscode-server
- cesta ke konfiguračnímu souboru: .vscode-server/data/Machine/
- sem vložit soubor: settings.json

```
{
  "files.watcherExclude": {
    "**/**": true
  },
  "files.exclude": {
    "**/.git": true,
    "**/node_modules": true,
    "**/*.log": true
  },
  "search.exclude": {
    "**/**": true
  },
  "search.useGlobalIgnoreFiles": false,
  "search.useParentIgnoreFiles": false,
  "extensions.ignoreRecommendations": true,
  "remote.SSH.showLoginTerminal": false,
  "git.enabled": false,
  "git.autorefresh": false,
  "editor.occurrencesHighlight": "off",
  "editor.codeLens": false,
  "files.autoSave": "off",
  "editor.formatOnSave": false,
  "editor.suggestOnTriggerCharacters": false,
  "telemetry.telemetryLevel": "off",
  "typescript.disableAutomaticTypeAcquisition": true,
  "javascript.validate.enable": false,
  "typescript.validate.enable": false,
  "editor.minimap.enabled": false,
  "editor.hover.enabled": false,
  "editor.lightbulb.enabled": "off",
  "editor.parameterHints.enabled": false,
  "editor.quickSuggestions": {
    "other": false,
    "comments": false,
    "strings": false
  }
}
```

### Další řádky by ještě mohly být:
```
"remote.SSH.useLocalServer": false    Zejména tento.
"remote.SSH.connectTimeout": 15000
"terminal.integrated.inheritEnv": false,
"terminal.integrated.enableFileWatching": false
```


