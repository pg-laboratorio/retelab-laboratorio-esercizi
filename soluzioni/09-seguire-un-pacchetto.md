# Soluzione – Esercizio 9

**Risultato atteso:** gli IP restano uguali dall'origine alla destinazione, mentre i MAC cambiano a ogni attraversamento del router.

## Gli indirizzi MAC

Con [`rete-esercizio-3.json`](../reti/rete-esercizio-3.json) i valori sono questi. Se hai costruito la rete da solo i MAC sono diversi, ma il ragionamento è identico.

| Nome | Dispositivo | MAC (Windows) | MAC (Cisco) |
| --- | --- | --- | --- |
| MAC-A1 | PC-A1 | 00-1A-0E-01-05-8C | 001a.0e01.058c |
| MAC-A2 | PC-A2 | 00-50-F6-C7-BD-46 | 0050.f6c7.bd46 |
| MAC-B1 | PC-B1 | 00-60-32-A2-45-E9 | 0060.32a2.45e9 |
| MAC-R1a | R1 G0/0 | 00-50-E2-45-A9-A0 | 0050.e245.a9a0 |
| MAC-R1b | R1 G0/1 | 00-60-DE-0E-11-F3 | 0060.de0e.11f3 |

## Tabelle compilate

Ping da PC-A1 a PC-A2:

| Tratto | MAC sorgente | MAC destinazione | IP sorgente | IP destinazione |
| --- | --- | --- | --- | --- |
| PC-A1 → Switch-A → PC-A2 | MAC-A1 | MAC-A2 | 192.168.10.10 | 192.168.10.11 |

Ping da PC-A1 a PC-B1:

| Tratto | MAC sorgente | MAC destinazione | IP sorgente | IP destinazione |
| --- | --- | --- | --- | --- |
| PC-A1 → R1 (richiesta) | MAC-A1 | MAC-R1a | 192.168.10.10 | 192.168.20.10 |
| R1 → PC-B1 (richiesta) | MAC-R1b | MAC-B1 | 192.168.10.10 | 192.168.20.10 |
| PC-B1 → R1 (risposta) | MAC-B1 | MAC-R1b | 192.168.20.10 | 192.168.10.10 |
| R1 → PC-A1 (risposta) | MAC-R1a | MAC-A1 | 192.168.20.10 | 192.168.10.10 |

L'errore più comune è scrivere MAC-B1 come destinazione già nel primo tratto. Chiediti: come farebbe PC-A1 a conoscere il MAC di un host che sta in un'altra rete?

Il pannello **Pacchetti** mostra le stesse righe. Nel primo ping compaiono anche le richieste ARP: PC-A1 chiede «chi ha 192.168.10.1?» e risponde R1; poi R1 chiede «chi ha 192.168.20.10?» e risponde PC-B1. Nella risposta PC-B1 non ha bisogno di chiedere: ha imparato il MAC di R1 dalla richiesta ARP che ha ricevuto.

## Risposte

1. No. Dopo il ping a PC-B1, `arp -a` su PC-A1 mostra 192.168.10.1 con il MAC dell'interfaccia del router (MAC-R1a). PC-A1 sa che PC-B1 è in un'altra rete, quindi non lo cerca nemmeno: chiede via ARP il MAC del gateway e consegna a lui la trama.
2. I MAC cambiano nel router, a ogni passaggio da una rete all'altra. Gli IP non cambiano mai lungo il percorso (in assenza di NAT).
3. Si scambiano: la sorgente della richiesta diventa la destinazione della risposta, sia per i MAC sia per gli IP.
4. Lo switch lavora a livello 2: inoltra la trama leggendo il MAC di destinazione, senza modificarla. Il router lavora a livello 3: legge l'IP, decide l'uscita e costruisce una trama nuova con i MAC della rete di destinazione. I MAC valgono solo dentro la rete locale, gli IP da un capo all'altro.

Su R1, `show arp` dopo il ping contiene sia PC-A1 sia PC-B1 (oltre alle sue due interfacce, con età «-»): è il router che conosce entrambe le reti.

```
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  192.168.10.1             -   0050.e245.a9a0  ARPA   GigabitEthernet0/0
Internet  192.168.10.10            0   001a.0e01.058c  ARPA   GigabitEthernet0/0
Internet  192.168.20.1             -   0060.de0e.11f3  ARPA   GigabitEthernet0/1
Internet  192.168.20.10            0   0060.32a2.45e9  ARPA   GigabitEthernet0/1
```
