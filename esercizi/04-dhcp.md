# Esercizio 4 – Indirizzi automatici con DHCP

**Tempo indicativo:** 40 minuti · **Argomenti:** DHCP, pool, esclusioni

## Obiettivo

Far assegnare gli indirizzi ai PC dal router invece di configurarli a mano.

## Rete di partenza

Parti dalla rete dell'[esercizio 3](03-due-lan-e-un-router.md): importa il file che hai salvato, oppure [`rete-esercizio-3.json`](../reti/rete-esercizio-3.json) (sulla pagina del file, pulsante **Download raw file**; poi in ReteLab **Importa → Apri un file .json**).

## Istruzioni

Configura sul router un pool per ciascuna LAN, poi imposta tutti i PC in DHCP.

| Pool | Rete | Gateway | Indirizzi esclusi |
| --- | --- | --- | --- |
| LAN_A | 192.168.10.0/24 | 192.168.10.1 | 192.168.10.1 – 192.168.10.9 |
| LAN_B | 192.168.20.0/24 | 192.168.20.1 | 192.168.20.1 – 192.168.20.9 |

Esempio per LAN A, dal **Terminale** di R1:

```
enable
configure terminal
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp pool LAN_A
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
end
```

Poi, su ogni PC, nella scheda **Configurazione** scegli **Automatica (DHCP)**. Se un PC aveva chiesto l'indirizzo prima che il pool esistesse, nel suo terminale scrivi `ipconfig /renew`.

## Prove da eseguire

1. Su ogni PC verifica con `ipconfig` l'indirizzo ricevuto e annotalo. Sul router confrontali con `show ip dhcp binding`.
2. Ripeti i ping dell'esercizio 3: devono funzionare tutti.
3. Aggiungi un terzo PC alla LAN B, mettilo in DHCP e verifica che riceva un indirizzo senza nessun'altra configurazione.

## Domande

1. Perché escludiamo i primi indirizzi del pool?
2. Quali informazioni riceve un PC dal DHCP oltre all'indirizzo IP?
3. Cosa succede se dimentichi la riga `default-router`? Provalo: entra nel pool con `ip dhcp pool LAN_B`, scrivi `no default-router`, poi `ipconfig /renew` su un PC della LAN B.

---

[← Esercizio precedente](03-due-lan-e-un-router.md) · [Soluzione](../soluzioni/04-dhcp.md) · [Esercizio successivo →](05-trova-i-guasti.md)
