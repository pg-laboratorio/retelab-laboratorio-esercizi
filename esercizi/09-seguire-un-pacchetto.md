# Esercizio 9 – Seguire un pacchetto: MAC e IP hop per hop

**Tempo indicativo:** 50 minuti · **Argomenti:** indirizzi MAC e IP, ARP · **Ispirato a:** Cisco 13.1.3 *Identify MAC and IP Addresses*

## Obiettivo

Capire quali indirizzi cambiano durante il viaggio di un pacchetto e quali restano uguali.

Usa la rete dell'[esercizio 3](03-due-lan-e-un-router.md) (due LAN e il router R1), funzionante: la tua oppure [`rete-esercizio-3.json`](../reti/rete-esercizio-3.json). Dopo l'importazione le tabelle ARP sono vuote.

## Fase 1 – Raccogli gli indirizzi MAC

Annota il MAC di PC-A1, PC-A2, PC-B1 e delle due interfacce di R1. Per comodità chiamali MAC-A1, MAC-A2, MAC-B1, MAC-R1a (interfaccia G0/0, LAN A) e MAC-R1b (interfaccia G0/1, LAN B).

- Sui PC: `ipconfig /all` (riga «Indirizzo fisico») oppure la scheda **Configurazione**.
- Sul router: `show interfaces` oppure la scheda **Interfacce**.

Attenzione: Windows scrive il MAC come `00-1A-0E-01-05-8C`, Cisco come `001a.0e01.058c`. È lo stesso indirizzo scritto in due modi.

## Fase 2 – Prevedi, su carta

Compila le tabelle **prima** di usare il simulatore.

Ping da PC-A1 a PC-A2 (stessa rete):

| Tratto | MAC sorgente | MAC destinazione | IP sorgente | IP destinazione |
| --- | --- | --- | --- | --- |
| PC-A1 → Switch-A → PC-A2 |  |  |  |  |

Ping da PC-A1 a PC-B1 (reti diverse), richiesta e risposta:

| Tratto | MAC sorgente | MAC destinazione | IP sorgente | IP destinazione |
| --- | --- | --- | --- | --- |
| PC-A1 → R1 (richiesta) |  |  |  |  |
| R1 → PC-B1 (richiesta) |  |  |  |  |
| PC-B1 → R1 (risposta) |  |  |  |  |
| R1 → PC-A1 (risposta) |  |  |  |  |

## Fase 3 – Verifica

1. Esegui i due ping da PC-A1.
2. Dopo ogni ping apri **Pacchetti**: mostra ogni tratto con MAC e IP, e le richieste ARP. Passando il mouse su un MAC vedi a quale dispositivo appartiene. Confronta tratto per tratto con le tue tabelle.
3. Controlla le tabelle ARP: `arp -a` su PC-A1 e `show arp` su R1.

## Domande

1. Nella tabella ARP di PC-A1, dopo il ping a PC-B1, compare il MAC di PC-B1? Quale MAC compare al suo posto, e perché?
2. In quale punto del percorso cambiano i MAC? Gli IP cambiano mai?
3. Cosa succede agli indirizzi nella risposta rispetto alla richiesta?
4. Perché lo switch non modifica nessun indirizzo, mentre il router riscrive i MAC?

---

[← Esercizio precedente](08-rete-per-reparti.md) · [Soluzione](../soluzioni/09-seguire-un-pacchetto.md) · [Esercizio successivo →](10-caccia-all-errore.md)
