# DYNO

## Einführung

DYNO ist eine dezentralisierte high-performance Blockchain, Gebaut von Fans von DYNO und KuCoin.

Entwickelt auf der Basis von "go-ethereum" mit dem Hintergedanken der Community eine "High-Speed" Blockchain Erfahrung mit niedrigen Gebühren zu bieten.

DYNO wird folgende Eigenschaften bieten:
- Voll kompatibel mit Ethereum und ERC-20 Smart Contracts und extrem niedrigen migrations Gebühren.
- Der KuCoin Token (DYNO) ist der native haupt Coin für die DYNO und wird unter anderem für Gas Gebühren genutzt.
- Ein Block wird alle 3 Sekunden berechnet was eine höhere TPS (Transactions per Second) und Chain performance ermöglicht.
- Die Basis der DYNO ist der "Proof of Staked Authority" (PoSA) Konsensusalgorithmus mit hoher Effizienz, Sicherheit und Stabilität.

## Mission

Mission: Beschleunigung der Wertübertragung um die ganze Welt ohne Grenzen.

# Netzwerk Parameter

Community Mitglieder können jede Ethereum compatible Wallet mit dem DYNO Netzwerk Parametern konfigurieren wie z.B. [metamask](https://metamask.io/), [myetherwallet](https://www.myetherwallet.com/), [imtoken](https://token.im/), [TokenPocket](https://www.tokenpocket.pro/) etc.

## Mainnet
```
Chain Name: DYNO Mainnet
Chain ID: 3966
Symbol: DYNO
RPC URL: https://api.dynoprotocol.com
Explorer URL: https://explorer.kcc.io/de
WebSocket RPC URL: wss://api.dynoprotocol.com
```

## Testnet
```
Chain Name: DYNO Testnet
Chain ID: 3967
Symbol: DYNO
RPC URL: https://tapi.dynoprotocol.com
Explorer URL: https://testnet.dynoscan.io
WebSocket RPC URL: wss://tapi.dynoprotocol.com

Faucet URL: https://faucet.dynoscan.io (Zu testzwecken ohne einen Wert)
```

# Developer Dokumentation

## Kompilierung
### Vorraussetzungen
- Linux oder Mac
- golang >= 1.13
- git

[Wie downloadet und installiert man golang?](https://golang.org/doc/install)

### Schritte
```
git clone -b kcc --single-branch https://github.com/dyno-protocol/go-ethereum.git
cd kcc
make geth
```
### Ausführen

Die Commandline Flags sind ähnlich wie beim originalen "go-ethereum", um alle verfügbaren Flags zu sehen nutze bitte `./build/bin/geth --help`.
Um zum Beispiel dem Testnet beizutreten nutze einfach `./build/bin/geth --testnet`.

## Docker

Auch bieten wir ein Docker Image [https://hub.docker.com/r/kucoincommunitychain/kcc](https://hub.docker.com/r/kucoincommunitychain/kcc) um ein schnelles Deployment zu ermöglichen.

[Wie nutze ich Docker?](https://docs.docker.com/get-started/)

## Deployment

### Vorraussetzungen
```
4 core cpu
8g memory
scalable SSD disk
public ip with TCP/UDP:30303 open
```

### Start command
```
./build/bin/geth #Mainnet
./build/bin/geth --testnet #Testnet

useful options:
/data/kcc/geth \
--datadir /data/.kcc/testnet \ #your data dir
--testnet \ #Testnet
--http \ #http rpc
--ws \ #ws rpc
--http.vhosts * \ #vhosts
--rpccorsdomain * \ #http corsdomain
--http.addr 0.0.0.0 \ #http rpc bind address
--ws.addr 0.0.0.0 \ #ws rpc bind address
--syncmode full \ #syncmode
--gcmode archive #gcmode
```

Um den Node (`geth`) im Hintergrund laufen zu lassen, kannst du `nohup`,`supervisor`,`systemd` nutzen.

- [supervisor](http://supervisord.org/)
- [systemd](https://wiki.debian.org/systemd)

## SDKs

Um mit dem DYNO Node (RPC oder Websocket) zu interagieren kannst du eine der folgenden SDKs nutzen.

- [Js: web3.js](https://github.com/kcc-community/web3.js) Ethereum JavaScript API
- [Java: web3j](https://github.com/web3j/web3j) Web3 Java Ethereum Ðapp API
- [PHP: web3.php](https://github.com/sc0Vu/web3.php) A php interface for interacting with the Ethereum blockchain and ecosystem.
- [Python: Web3.py](https://github.com/ethereum/web3.py) A Python library for interacting with Ethereum, inspired by web3.js.
- [Golang: go-ethereum](https://github.com/ethereum/go-ethereum)

## Konsensusalgorithmus

Die DYNO nutzt den PoSA Konsensusalgorithmus welcher es ermöglicht niedrige Transaktiosgebühren, Transaktionswartezeiten sowie eine hohe Transaktionsparallelität zu ermöglichen und unterstützt damit bis zu maximal 29 Validatoren.

PoSA ist eine Kombination auf PoA und PoS. Um ein Validator zu werden musst du ein "proposal/Anfrage" dafür einreichen und dann warten bis aktive Validatoren über deine Anfrage abgestimmt haben. Nachdem mehr als die hälfte der aktiven Validatoren abgestimmt haben ist es dir erlaubt Validator zu werden. Jeglicher Adresse kann seine DYNO einer anderen Addresse "staken" welche qualifiziert ist ein Validator zu werden. Nachdem das Staking Volumen des jeweiligen Nodes die Top 29 Staking Addressen erreicht wird ein aktiver Validator im nächsten Epoch solange er die Position in den Top 29 hält.

Alle aktiven Validatoren werden dazu angehalten anhand vordefinierter Regeln Blöcke zu "minen/berrechnen". Wenn ein Validator bei der berrechung eines Blocks dran ist, versagt, wird der Validator der nicht involviert war in den letzten "n/2" (n ist die Nummer of aktiven Validatoren) Blöcken wird zufallsbasiert einen "block-out" durchführen. Es müssen mindestens "n/2+1" aktive Validatoren sicher arbeiten um die Operationssicherheit der Blockchain zu gewährleisten.

Die schwierigkeit eines Block ist 2 wenn der Block automatisch generiert ist, und 1 wenn der Block nicht durch eine vordefinierte Order generiert worden ist. Wenn ein fork der Blockchain erscheint, wählt die Blockchain den zugehörigen Frok entsprechend der kumulierten maximalen Schwierigkeit.


### System Contracts

DYNO hat 3 integrierte System Contracts für den PoSA in der Geniss Datei.

Der Source Code dieser 3 Contracts sind von "Heco" gefroked und sind auf unserem Github Repository zu finden: [https://github.com/kcc-community/kcc-genesis-contracts](https://github.com/kcc-community/kcc-genesis-contracts)。

Die Verwaltung der aktuellen Validatoren obliegt den System Contracts.

- Verantwortlich für Zugriffsrechte auf Validatoren und verwaltung von Validator Anträgen sowie Votes/Stimmen.
- Verantwortlich für Rang Management von Validatoren swoie Staking und Unstaking Operationen und die verteilung der Block Rewards etc.
- Verantwortlich für Bestrafung aktiver Validatoren welche nicht zuverlässlich arbeiten.

Blockchain System Contract aufrufe：

- Am Ende jeden Blocks wird der Validator Contract aufgerufen und die errechneten Gebühren für alle Transaktionen innerhalb des Blocks werden auf alle aktiven Validatoren verteilt.
- Der Bestrafungs Contract wird dann aufgerufen wenn ein Validator nicht zuverlässlich arbeitet.
- Am Ende jeden Epochs wird der Validator Contract aufgerufen um die aktiven Validatoren liste anhand des Rankings zu updaten.

### stake

Du kannst die `stake` Methode im `validator` Contract aufrufen um für egal welchen Validator zu staken, das minimum das für einen Validator gestaked werden muss beträgt 32 DYNO.

### unstake

Wenn du dein DYNO unstaken möchtest dann musst du die `unstake` Methode im `validator` Contract aufrufen und für mindestens 86400 Blöcke (4 Tage) warten um danach die Methode `withdrawStaking` im `validator` Contract  aufrufen zu können um deine DYNO zu erhalten.

### punish

Wannimmer ein Validator gefunden wird der nicht zuverlässlich einen Block verarbeitet wie vorgeschrieben, wird der Bestrafungs Contract automatisch aufgerufen. Am Ende des jeweiligen Block wird dieser Aufruf gezählt, sollten die gezählten Aufrufe "24" erreichen so wird dem Validator sein gesammtes Einkommen entzogen, sollte die Anzahl auf 48 steigen wird der aktive Validator aus der Gruppe der aktiven Validatoren entfernt und ist als dieser disqualifiziert.

# Governance

## Advice, Issue & Discussion

Jegliche Tipps, Anregungen und Diskussionen sind Willkommen.

[Hinterlasse einen Tipp/Anmerkung oder Fehler/Problem](https://github.com/kcc-community/any-advice-issue/issues)

[Starte eine Diskussion](https://github.com/kcc-community/any-advice-issue/discussions)

Wenn du ein Problem mit einem speziellen Projekt hast dann verschiebe dieses `issue` in das jeweilige spezial Projekt.

## KIPs

DYNO Verbesserungsvorschläge

DYNO Verbesserungsvorschläge (KIPs)  beschreiben den Standard für die DYNO Plattform inkludiert Chain, Dex and dApps.

Der Idee hinter diesem Prozess ist es alle änderungen an der DYNO so transparent und demokratisch wie möglich zu gestalten.

URL：[https://github.com/kcc-community/KIPs](https://github.com/kcc-community/KIPs)


# FAQ
## MetaMask

Nutze den Chrome Browser und öffne MetaMask [extension site](https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn?hl=zh-CN)

Folge den Anweisungem um deine ETH Wallet zu erzeugen und **Backuppe deinen Private Key und/oder deine Memonic**

Konfiguriere MetaMask für das DYNO Mainnet

(1) Öffne MetaMask. Im oberen Bereich siehst du das standard Netzwerk 【Ethereum mainnet】。

<img width="170" alt="E1" src="https://user-images.githubusercontent.com/13411690/121641021-3f093900-cac1-11eb-9c06-fd653cd598a1.png">

Klicke auf【Ethereum mainnet】danach klicke auf【custom RPC】im Dropdown Menü.

<img width="170" alt="E2" src="https://user-images.githubusercontent.com/13411690/121641049-4597b080-cac1-11eb-8674-3755c30a3398.png">

(2) Fülle die Felder in folgender reihenfolge aus um das DYNO Mainnet hinzuzufügen:
    Network Name：DYNO-MAINNET

    New RPC URL：https://api.dynoprotocol.com
    
    Chain ID: 3966
    
    Currency Symbol (optional)：DYNO
    
    Block Explorer URL (optional):https://dynoscan.io

<img width="170" alt="E3" src="https://user-images.githubusercontent.com/13411690/121641889-598fe200-cac2-11eb-92c5-6617c103ebee.png">

Fertig

<img width="170" alt="E4" src="https://user-images.githubusercontent.com/13411690/121641085-51837280-cac1-11eb-80cd-1a208c0bcd54.png">
