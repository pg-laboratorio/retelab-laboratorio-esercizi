# Soluzione – Esercizio 1

**Risultato atteso:** il ping da PC1 a PC2 riesce. Dopo aver spostato PC2 in 192.168.2.11 fallisce.

## Svolgimento

1. Da un progetto vuoto aggiungi uno switch e due PC.
2. Con lo strumento **Cavo** collega PC1 allo switch, poi PC2. Sui cavi compaiono i nomi delle porte (per esempio Fa0/1 ed eth0) e un pallino verde: il collegamento è attivo.
3. Configura PC1: IP 192.168.1.10, mask 255.255.255.0. Il gateway non serve. In ReteLab puoi anche scrivere `192.168.1.10/24` nel campo dell'indirizzo: la mask si compila da sola.
4. Configura PC2 con IP 192.168.1.11.
5. Dal terminale di PC1 esegui `ping 192.168.1.11`.

## Risposte

1. Sì:

   ```
   Esecuzione di Ping 192.168.1.11 con 32 byte di dati:
   Risposta da 192.168.1.11: byte=32 durata<1ms TTL=128
   Risposta da 192.168.1.11: byte=32 durata<1ms TTL=128
   Risposta da 192.168.1.11: byte=32 durata<1ms TTL=128
   Risposta da 192.168.1.11: byte=32 durata<1ms TTL=128

   Statistiche Ping per 192.168.1.11:
       Pacchetti: Trasmessi = 4, Ricevuti = 4, Persi = 0 (0% persi)
   ```

   Sullo schema il pacchetto parte da PC1, attraversa lo switch e torna indietro.
2. No. Lo switch lavora a livello 2: legge l'indirizzo MAC di destinazione di ogni trama e la inoltra sulla porta giusta, consultando la propria tabella MAC. Un IP servirebbe solo per gestirlo da remoto. Infatti in ReteLab lo switch non ha nessun campo per l'indirizzo IP.
3. Il ping fallisce con quattro «Richiesta scaduta». PC1 confronta la destinazione con la propria rete: 192.168.2.11 con mask /24 appartiene a 192.168.2.0, non a 192.168.1.0. PC1 dovrebbe affidare il pacchetto al gateway, ma il gateway non c'è. Il pacchetto non parte nemmeno, anche se i due PC sono fisicamente sullo stesso switch: sullo schema non si muove nulla.

## Errori frequenti

Scrivere lo stesso IP su entrambi i PC (`ipconfig` lo segnala con «(Duplicato)»), dimenticare un cavo, usare come mask 255.255.255.255.
