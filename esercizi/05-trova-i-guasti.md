# Esercizio 5 – Trova i guasti

**Tempo indicativo:** 45 minuti · **Argomenti:** troubleshooting

## Obiettivo

Individuare e correggere errori di configurazione con un metodo ordinato.

## Scenario

Un collega ha configurato la rete dell'[esercizio 3](03-due-lan-e-un-router.md), ma qualcosa non va: alcuni PC non comunicano. **La rete contiene cinque errori.**

Importa [`05-trova-i-guasti.json`](../reti/05-trova-i-guasti.json) (pulsante **Download raw file**, poi in ReteLab **Importa → Apri un file .json**). La rete si apre in **modalità esercizio**: non c'è il pulsante Verifica rete, il terminale non spiega perché un ping fallisce e sotto i PC non compaiono gli indirizzi. Gli errori si trovano solo con i comandi, come su una rete vera. Non disattivare la modalità esercizio.

## Come dovrebbe essere la rete

Questo è il progetto corretto. Confrontalo con quello che trovi davvero sui dispositivi.

| Dispositivo | Interfaccia | Indirizzo IP | Subnet mask | Gateway |
| --- | --- | --- | --- | --- |
| R1 | G0/0 (verso LAN A) | 192.168.10.1 | 255.255.255.0 | — |
| R1 | G0/1 (verso LAN B) | 192.168.20.1 | 255.255.255.0 | — |
| PC-A1 | — | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC-A2 | — | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC-B1 | — | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |
| PC-B2 | — | 192.168.20.11 | 255.255.255.0 | 192.168.20.1 |

## Metodo consigliato, dal basso verso l'alto

1. **Collegamenti:** i cavi sono collegati e le interfacce del router accese? Sul router: `show ip interface brief`.
2. **Indirizzi:** ogni PC è nella rete giusta, con mask corretta e IP unico? Su ogni PC: `ipconfig`.
3. **Gateway:** il gateway di ogni PC è l'indirizzo del router nella sua LAN?
4. **Verifica:** dopo ogni correzione ripeti il ping per confermare.

## Registro dei guasti

| N. | Dispositivo | Errore trovato | Sintomo osservato | Correzione |
| --- | --- | --- | --- | --- |
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

## Domanda finale

Quale errore è stato il più difficile da trovare e quale comando ti ha aiutato?

---

[← Esercizio precedente](04-dhcp.md) · [Soluzione](../soluzioni/05-trova-i-guasti.md) · [Esercizio successivo →](06-lan-nuova-filiale.md)
