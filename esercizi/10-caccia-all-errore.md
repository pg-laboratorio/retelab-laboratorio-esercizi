# Esercizio 10 – Caccia all'errore a squadre

**Tempo indicativo:** due ore (un'ora per turno) · **Argomenti:** troubleshooting · **Ispirato a:** Cisco 17.1.3 *Use the ipconfig Command* e 17.1.6 *Use the ping Command*

## Obiettivo

Usare `ping` e `ipconfig` come un tecnico, su guasti inventati da un'altra squadra.

La classe lavora in coppie di squadre, A e B.

## Turno 1 – La squadra A sabota

1. La squadra A prepara una rete funzionante, a scelta tra quella dell'[esercizio 3](03-due-lan-e-un-router.md) (si può importare [`rete-esercizio-3.json`](../reti/rete-esercizio-3.json)) e quella dell'[esercizio 6](06-lan-nuova-filiale.md). Verifica che tutti i ping riescano.
2. Poi introduce **esattamente due errori**, scelti dall'elenco sotto. Li scrive su un foglio che consegna al docente, senza mostrarlo alla squadra B.
3. Apre **Esporta**, attiva **Modalità esercizio** e scarica il file con il nome `SquadraA_Turno1`. Così chi lo apre non vede Verifica rete, le spiegazioni dei guasti e gli indirizzi sotto i PC.
4. La squadra B importa il file (o si siede alla postazione) e ha 30 minuti per trovare e correggere i guasti.

### Errori ammessi

- indirizzo IP di un PC in una rete sbagliata;
- subnet mask sbagliata;
- gateway sbagliato o mancante;
- indirizzo IP duplicato;
- interfaccia del router spenta;
- pool DHCP con il gateway (`default-router`) sbagliato (solo sulla rete dell'esercizio 6).

Non si cancellano dispositivi e non si spostano cavi: il guasto deve essere di configurazione.

## Regole per chi ripara

Prima di cambiare qualcosa bisogna scrivere il sintomo osservato e il comando che lo dimostra. Una correzione «al buio», senza diagnosi, non vale punti. Non si disattiva la modalità esercizio.

| Guasto | Sintomo osservato | Comando che lo dimostra | Correzione |
| --- | --- | --- | --- |
| 1 |  |  |  |
| 2 |  |  |  |

## Punteggio

- 2 punti per ogni guasto corretto con diagnosi completa;
- 1 punto se corretto ma con diagnosi incompleta;
- la squadra che ha sabotato prende 1 punto per ogni guasto non trovato entro il tempo.

## Turno 2

Le squadre si scambiano i ruoli.

## Domanda finale

Quale dei due guasti che avete ricevuto si vedeva subito, e quale era nascosto? Da cosa dipende?

---

[← Esercizio precedente](09-seguire-un-pacchetto.md) · [Soluzione](../soluzioni/10-caccia-all-errore.md) · [Torna all'indice](../README.md)
