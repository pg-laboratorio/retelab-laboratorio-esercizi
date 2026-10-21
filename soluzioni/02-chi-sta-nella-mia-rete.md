# Soluzione – Esercizio 2

**Risultato atteso:** da PC1 riesce solo il ping verso PC2. PC3 e PC4 non rispondono. Con la /16 su PC1 e PC3, anche PC3 risponde.

## Come si calcola l'indirizzo di rete

Si fa l'AND bit a bit tra indirizzo e mask: dove la mask ha 1 il bit resta, dove ha 0 diventa 0. Con le mask «tonde», un ottetto 255 si copia e un ottetto 0 diventa 0.

Esempio per PC3, terzo ottetto:

```
IP     10.0.1.20      ->  terzo ottetto 00000001
Mask   255.255.255.0  ->  terzo ottetto 11111111
AND                   ->  terzo ottetto 00000001 = 1
Rete   10.0.1.0
```

| PC | IP / mask | Indirizzo di rete | Ping da PC1 | Perché |
| --- | --- | --- | --- | --- |
| PC1 | 10.0.0.1 /24 | 10.0.0.0 | — | — |
| PC2 | 10.0.0.50 /24 | 10.0.0.0 | riesce | Stessa rete di PC1 |
| PC3 | 10.0.1.20 /24 | 10.0.1.0 | fallisce | Rete diversa, nessun gateway |
| PC4 | 10.0.1.30 /16 | 10.0.0.0 | fallisce | Per PC1 è in un'altra rete |

## Risposte

1. Vedi la tabella.
2. Lo switch inoltrerebbe senza problemi, ma il ping non parte. PC1 calcola che 10.0.1.20 non è nella rete 10.0.0.0/24, quindi lo affiderebbe al gateway, che non esiste. La decisione si prende nel PC, prima ancora che il pacchetto arrivi allo switch. Il terminale di ReteLab lo spiega nella riga «→ Perché».
3. Sì. Con mask 255.255.0.0 entrambi calcolano la rete 10.0.0.0, quindi si considerano vicini e comunicano direttamente.
4. Una /24 ha 2⁸ − 2 = 254 host utilizzabili; una /16 ne ha 2¹⁶ − 2 = 65.534. Si tolgono l'indirizzo di rete e quello di broadcast.

## Il caso PC4

PC4, con la sua /16, vede PC1 nella propria rete. Ma PC1, con la /24, considera PC4 esterno e non sa come rispondergli. Provalo: il ping da PC4 a PC1 fallisce lo stesso, e il terminale spiega che la richiesta è arrivata ma la risposta si è persa. Una comunicazione funziona solo se **entrambi** i lati sono d'accordo: è la conseguenza di mask incoerenti nella stessa LAN.
