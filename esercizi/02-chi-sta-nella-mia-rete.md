# Esercizio 2 – Chi sta nella mia rete?

**Tempo indicativo:** 30 minuti · **Argomenti:** subnet mask, indirizzo di rete

## Obiettivo

Capire come la subnet mask decide quali host sono raggiungibili direttamente.

## Istruzioni

1. Parti da un progetto vuoto. Aggiungi uno switch e quattro PC, tutti collegati allo switch.
2. Configura gli indirizzi della tabella. Fai attenzione: le mask non sono tutte uguali.
3. Prima di provare, compila la colonna «Previsione»: PC1 riuscirà a pingare quel PC?
4. Esegui i ping da PC1 verso gli altri tre PC e compila la colonna «Risultato».

| Dispositivo | Indirizzo IP | Subnet mask | Previsione (sì/no) | Risultato |
| --- | --- | --- | --- | --- |
| PC1 | 10.0.0.1 | 255.255.255.0 | — | — |
| PC2 | 10.0.0.50 | 255.255.255.0 |  |  |
| PC3 | 10.0.1.20 | 255.255.255.0 |  |  |
| PC4 | 10.0.1.30 | 255.255.0.0 |  |  |

## Domande

1. Calcola l'indirizzo di rete di ciascun PC.
2. Perché PC1 e PC3 non comunicano, anche se sono sullo stesso switch?
3. Cambia la mask di PC1 e PC3 in 255.255.0.0. Il ping ora funziona? Spiega.
4. Quanti host utilizzabili ha una rete /24? E una /16?

---

[← Esercizio precedente](01-due-pc-e-uno-switch.md) · [Soluzione](../soluzioni/02-chi-sta-nella-mia-rete.md) · [Esercizio successivo →](03-due-lan-e-un-router.md)
