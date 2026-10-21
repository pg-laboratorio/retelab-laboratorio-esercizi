# Soluzione – Esercizio 5

**Risultato atteso:** cinque errori trovati e corretti, poi tutti i PC si pingano a vicenda. Conta il metodo più della lista finale.

## Passo 1 – Collegamenti e interfacce

Sullo schema il cavo tra R1 e Switch-B è tratteggiato, con un pallino rosso: il collegamento non funziona. Su R1 `show ip interface brief` lo conferma:

```
GigabitEthernet0/1     192.168.20.1    YES manual administratively down down
```

Correzione, dal terminale di R1:

```
enable
configure terminal
interface g0/1
 no shutdown
end
```

Sintomo: nessun PC della LAN B raggiunge il proprio gateway, e dalla LAN A non si raggiunge nulla della LAN B. Va corretto per primo: finché l'interfaccia è spenta, ogni test sulla LAN B fallisce e nasconde gli altri errori.

## Passo 2 – Indirizzi dei PC

- **PC-A2** ha 192.168.10.10, lo stesso IP di PC-A1. `ipconfig` mostra «192.168.10.10(Duplicato)». Sintomo: ping instabili, con un numero di risposte che cambia da una prova all'altra, perché a volte risponde il PC sbagliato. Correzione: 192.168.10.11.
- **PC-B2** ha 192.168.21.11/24, quindi è nella rete 192.168.21.0. Il suo gateway 192.168.20.1 sta in un'altra rete e non è raggiungibile: PC-B2 non comunica con nessuno. Correzione: 192.168.20.11.
- **PC-B1** ha mask 255.255.0.0, quindi si crede nella rete 192.168.0.0/16, che contiene anche 192.168.10.x. Sintomo: PC-B1 pinga il gateway e la propria LAN, ma non la LAN A, perché cerca PC-A1 direttamente invece di passare dal router. Correzione: 255.255.255.0.

## Passo 3 – Gateway

PC-A1 ha gateway 192.168.10.254, un indirizzo che nessun dispositivo possiede. Sintomo caratteristico: PC-A1 pinga 192.168.10.1, perché è nella sua rete, ma non le reti remote. Quando il router risponde ma le reti remote no, controlla il gateway configurato. Correzione: 192.168.10.1.

## Passo 4 – Verifica finale

Prova almeno PC-A1 ↔ PC-A2, PC-B1 ↔ PC-B2, PC-A1 ↔ PC-B1, PC-A2 ↔ PC-B2: devono rispondere tutti 4 su 4.

## Registro compilato

| N. | Dispositivo | Errore | Sintomo | Correzione |
| --- | --- | --- | --- | --- |
| 1 | R1 | Interfaccia G0/1 in shutdown | LAN B irraggiungibile | `no shutdown` |
| 2 | PC-A2 | IP duplicato 192.168.10.10 | Ping instabili nella LAN A, «(Duplicato)» in `ipconfig` | IP 192.168.10.11 |
| 3 | PC-B2 | IP 192.168.21.11 fuori rete | PC-B2 isolato | IP 192.168.20.11 |
| 4 | PC-B1 | Mask 255.255.0.0 | LAN B ok, LAN A irraggiungibile | Mask 255.255.255.0 |
| 5 | PC-A1 | Gateway 192.168.10.254 | Gateway locale ok, reti remote no | Gateway 192.168.10.1 |

La tua numerazione può essere diversa: conta che ogni riga colleghi correttamente errore, sintomo e correzione. Il guasto più subdolo è di solito la mask di PC-B1, perché la LAN B funziona e il problema compare solo verso la LAN A.

Per il docente: disattivando la modalità esercizio (clic sull'etichetta in alto) il pulsante **Verifica rete** elenca i guasti rimasti.
