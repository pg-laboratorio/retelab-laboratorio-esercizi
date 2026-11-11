# Soluzione – Esercizio 11

**Risultato atteso:** dopo il primo ping ogni switch conosce PC-Amm1 e PC-Ven2, sulla porta da cui li ha «sentiti». Dopo lo svuotamento, SW-Finanza manda il primo frame su tutte le porte e poi impara di nuovo.

## Fase 1

Le tre tabelle sono vuote:

```
Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
Total Mac Addresses for this criterion: 0
```

Uno switch appena acceso non sa nulla: impara solo quando passa del traffico.

## Fase 2 e 3 – Tabelle dopo il ping

Con la rete fornita, PC-Amm1 ha MAC `001a.0e01.058c` e PC-Ven2 ha MAC `000c.7e84.c953`.

| Switch | Dispositivo (MAC) | Porta | Da quale frame l'ha imparato |
| --- | --- | --- | --- |
| SW-Amministrazione | PC-Amm1 (001a.0e01.058c) | Fa0/1 | richiesta ARP di PC-Amm1 |
| SW-Amministrazione | PC-Ven2 (000c.7e84.c953) | Fa0/8 | risposta ARP di PC-Ven2 |
| SW-Finanza | PC-Amm1 (001a.0e01.058c) | Fa0/7 | richiesta ARP di PC-Amm1 |
| SW-Finanza | PC-Ven2 (000c.7e84.c953) | Fa0/8 | risposta ARP di PC-Ven2 |
| SW-Vendite | PC-Amm1 (001a.0e01.058c) | Fa0/7 | richiesta ARP di PC-Amm1 |
| SW-Vendite | PC-Ven2 (000c.7e84.c953) | Fa0/2 | risposta ARP di PC-Ven2 |

Per esempio, su SW-Finanza:

```
Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    001a.0e01.058c    DYNAMIC     Fa0/7
   1    000c.7e84.c953    DYNAMIC     Fa0/8
Total Mac Addresses for this criterion: 2
```

Cosa succede, frame per frame:

1. **Richiesta ARP** (broadcast, destinazione FF-FF-FF-FF-FF-FF). Entra in SW-Amministrazione da Fa0/1: lo switch impara PC-Amm1 su Fa0/1 e, trattandosi di un broadcast, la manda su tutte le altre porte. SW-Finanza la riceve da Fa0/7 e impara PC-Amm1 su Fa0/7; SW-Vendite la riceve da Fa0/7 e fa lo stesso. Tutti e tre conoscono PC-Amm1.
2. **Risposta ARP** (unicast, da PC-Ven2 a PC-Amm1). Entra in SW-Vendite da Fa0/2: impara PC-Ven2. Gli switch conoscono già PC-Amm1, quindi la inoltrano solo verso di lui: Fa0/7 su SW-Vendite e SW-Finanza, Fa0/1 su SW-Amministrazione. Lungo la strada imparano PC-Ven2 sulla porta da cui arriva.
3. **Ping** (richiesta e risposta ICMP). Tutti i MAC sono già noti: nessuna riga nuova, nessun flooding.

## Fase 4 – Il flooding

Dopo `clear mac address-table dynamic` su SW-Finanza, il pannello **Pacchetti** riporta:

| Frame | SW-Amministrazione | SW-Finanza | SW-Vendite |
| --- | --- | --- | --- |
| Richiesta ICMP (PC-Amm1 → PC-Ven2) | Fa0/1 → Fa0/8 | Fa0/7 → **tutte le porte** (flooding) | Fa0/7 → Fa0/2 |
| Risposta ICMP (PC-Ven2 → PC-Amm1) | Fa0/8 → Fa0/1 | Fa0/8 → Fa0/7 | Fa0/2 → Fa0/7 |

Sullo schema gli anelli viola compaiono su SW-Finanza, PC-Fin1, PC-Fin2 e SW-Vendite. La tabella di SW-Finanza, alla fine, è di nuovo quella della fase 3.

## Risposte

1. Uno switch impara solo dai frame che riceve, e solo il MAC **sorgente**. In questo ping hanno trasmesso soltanto PC-Amm1 e PC-Ven2: gli altri quattro PC non hanno inviato nulla, quindi nessuno switch sa dove siano. Se provi un ping da PC-Fin1, compariranno anche loro.
2. La tabella non dice dove si trova fisicamente un dispositivo, ma da quale porta lo si raggiunge. Per SW-Finanza, PC-Amm1 sta «dietro» la porta Fa0/7, quella verso SW-Amministrazione: i suoi frame arrivano da lì. Su una porta tra due switch si accumulano quindi molti MAC.
3. SW-Finanza, che aveva appena svuotato la tabella e non sapeva dove fosse PC-Ven2. Ha mandato una copia del frame su tutte le altre porte della VLAN: a PC-Fin1, a PC-Fin2 e a SW-Vendite. PC-Fin1 e PC-Fin2 l'hanno ricevuto e scartato, perché il MAC di destinazione non era il loro. SW-Vendite conosceva PC-Ven2 e l'ha inoltrato solo su Fa0/2.
4. Perché il frame della richiesta ha insegnato a SW-Finanza dove sta PC-Amm1 (porta Fa0/7). Quando arriva la risposta, diretta a PC-Amm1, lo switch conosce già la porta e la inoltra solo lì. Nello stesso momento impara PC-Ven2 su Fa0/8.
5. L'apprendimento automatico, insieme al flooding: lo switch parte senza sapere nulla, manda su tutte le porte i frame per destinazioni sconosciute e intanto annota i MAC sorgente. Dopo pochi scambi conosce la rete e inoltra ogni frame solo dove serve. Per questo uno switch semplice si collega e funziona senza configurazione.

## Collegamento con l'esercizio 9

Nell'esercizio 9 hai visto che lo switch non modifica gli indirizzi. Qui vedi come decide dove mandare il frame: legge il MAC di **destinazione** e lo cerca nella sua tabella. Il router invece legge l'IP e riscrive i MAC.
