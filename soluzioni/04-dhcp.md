# Soluzione – Esercizio 4

**Risultato atteso:** ogni PC riceve un indirizzo da .10 in su nella propria rete, con il gateway corretto, e tutti i ping dell'esercizio 3 funzionano.

## Svolgimento

1. Parti dalla rete dell'esercizio 3. Le interfacce del router devono restare configurate: il DHCP non le sostituisce.
2. Configura il pool LAN_A con i comandi del testo, poi il pool LAN_B:

   ```
   configure terminal
   ip dhcp excluded-address 192.168.20.1 192.168.20.9
   ip dhcp pool LAN_B
    network 192.168.20.0 255.255.255.0
    default-router 192.168.20.1
   end
   ```

3. Su ogni PC scegli **Automatica (DHCP)**: i campi di indirizzo, mask e gateway diventano di sola lettura e si riempiono da soli.
4. Controlla gli indirizzi con `ipconfig` sui PC e con `show ip dhcp binding` sul router.
5. Ripeti i ping, poi aggiungi un PC alla LAN B e mettilo subito in DHCP.

Lo stesso risultato si ottiene dal pannello **DHCP** del router: sezione «Indirizzi esclusi» e pool con rete, mask e gateway.

## Come il router sceglie il pool

Il router non deve sapere quale PC sta in quale LAN. La richiesta DHCP arriva su un'interfaccia, e il router usa il pool la cui rete contiene l'indirizzo di quell'interfaccia. Una richiesta arrivata su 192.168.10.1 riceve un indirizzo di LAN_A. Nel pannello DHCP ogni pool mostra l'interfaccia che serve («serve G0/0»).

| PC | Indirizzo ricevuto (tipico) | Gateway |
| --- | --- | --- |
| PC-A1 | 192.168.10.10 | 192.168.10.1 |
| PC-A2 | 192.168.10.11 | 192.168.10.1 |
| PC-B1 | 192.168.20.10 | 192.168.20.1 |
| PC-B2 | 192.168.20.11 | 192.168.20.1 |
| Nuovo PC | 192.168.20.12 | 192.168.20.1 |

L'ordine di assegnazione dipende da quale PC chiede per primo: vanno bene tutti gli indirizzi nella rete giusta e non sotto .10.

## Risposte

1. Gli indirizzi da .1 a .9 sono riservati ai dispositivi con IP statico: il router (.1) e, in una rete reale, stampanti, server, access point. Senza esclusione il DHCP potrebbe assegnarli a un PC e creare un conflitto.
2. Subnet mask, gateway predefinito e, se configurato, il server DNS. Si vedono con `ipconfig /all`, che mostra anche l'indirizzo del server DHCP.
3. Dopo `no default-router` e `ipconfig /renew`, il PC riceve IP e mask ma nessun gateway: il terminale avvisa «il server non ha comunicato nessun gateway». Il PC comunica dentro la propria LAN, ma non con l'altra. Rimetti `default-router 192.168.20.1` e ripeti `ipconfig /renew`.

## Se un PC riceve un indirizzo 169.254.x.x

Non ha avuto risposta dal DHCP (indirizzo APIPA). Controlla che l'interfaccia del router sia accesa (`show ip interface brief`) e che la rete del pool coincida con quella dell'interfaccia (`show ip dhcp pool`), poi `ipconfig /renew`.
