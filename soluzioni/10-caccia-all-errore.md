# Soluzione – Esercizio 10

Non c'è una soluzione unica: ogni squadra riceve guasti diversi. Questa tabella riassume il sintomo tipico di ogni errore ammesso e il comando che lo dimostra.

| Errore | Sintomo | Comando che lo dimostra |
| --- | --- | --- |
| IP in una rete sbagliata | Il PC non pinga né il gateway né gli altri host | `ipconfig`: l'indirizzo non appartiene alla rete del gateway |
| Subnet mask sbagliata | Alcuni host raggiungibili e altri no, in modo apparentemente casuale | `ipconfig`: mask diversa da quella degli altri PC |
| Gateway sbagliato o mancante | LAN locale ok, reti remote irraggiungibili | `ping` al gateway corretto riesce, `ipconfig` mostra un gateway diverso |
| IP duplicato | Ping instabili, risposte dal dispositivo sbagliato | `ipconfig`: «(Duplicato)» accanto all'indirizzo; `arp -a`: il MAC cambia |
| Interfaccia del router spenta | Un'intera LAN non raggiunge il proprio gateway | `show ip interface brief`: «administratively down» |
| Pool DHCP con gateway sbagliato | Tutti i PC in DHCP comunicano in locale ma non verso l'esterno | `ipconfig` su più PC: tutti con lo stesso gateway errato; sul router `show ip dhcp pool` o `show running-config` |

## Il caso del pool DHCP

È il guasto più istruttivo. Viene spontaneo correggere il gateway sui PC, ma in DHCP i campi sono di sola lettura e il problema è sul router. La diagnosi completa è: «tutti i PC hanno lo stesso gateway errato, quindi viene dal DHCP». Si corregge nel pool (`default-router` giusto) e poi `ipconfig /renew` sui PC. Correggere i PC uno per uno, passandoli a indirizzo statico, vale solo il punto parziale.

## Domanda finale

Si vedono subito i guasti che bloccano tutto (interfaccia spenta, IP fuori rete). Restano nascosti quelli che colpiscono solo alcune comunicazioni (mask sbagliata, gateway sbagliato quando si prova solo in locale). Da qui la regola pratica: verificare sempre sia il traffico locale sia quello verso le altre reti.

## Note per il docente

- Il file scaricato dalla squadra A con la modalità esercizio attiva si apre già senza Verifica rete, spiegazioni e indirizzi sotto i PC.
- Per controllare velocemente il lavoro della squadra B, importa il file corretto, clicca l'etichetta «Modalità esercizio» per disattivarla e usa **Verifica rete**: se non segnala nulla e i ping riescono, i guasti sono stati corretti.
