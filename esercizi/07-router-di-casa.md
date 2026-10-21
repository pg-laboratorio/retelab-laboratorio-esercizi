# Esercizio 7 – Configurare il router di casa

**Tempo indicativo:** 40 minuti · **Argomenti:** calcolo del pool DHCP · **Ispirato a:** Cisco 11.2.3 *Configure DHCP on a Wireless Router*

## Obiettivo

Tradurre le impostazioni di un router domestico («indirizzo di partenza» e «numero massimo di utenti») nella configurazione DHCP di un router.

## Scenario

Un cliente vuole collegare tre PC al router di casa. Chiede che:

- il router abbia indirizzo 192.168.5.1/24;
- il DHCP assegni indirizzi a partire da 192.168.5.126;
- al massimo 75 dispositivi possano ricevere un indirizzo.

Nei router domestici queste opzioni si impostano da una pagina web. Su un router con comandi in stile Cisco, invece, non esiste un campo «numero massimo di utenti»: il pool si limita **escludendo** gli indirizzi che non vogliamo assegnare.

## Prima, su carta

1. Qual è l'ultimo indirizzo che il DHCP potrà assegnare?
2. Quali intervalli di indirizzi vanno esclusi?

## Poi, nel simulatore

1. Collega tre PC a uno switch e lo switch al router.
2. Assegna all'interfaccia G0/0 del router l'indirizzo 192.168.5.1/24 e accendila.
3. Configura le esclusioni che hai calcolato con `ip dhcp excluded-address`, poi il pool con `network` e `default-router`.
4. Imposta i tre PC in DHCP e annota gli indirizzi ricevuti.
5. Da un PC pinga il router e gli altri due PC.

## Domande

1. Gli indirizzi ricevuti confermano il tuo calcolo? Controlla anche con `show ip dhcp pool` sul router.
2. Cosa succederebbe al 76° dispositivo che si collega? Per provarlo senza 76 PC, aggiungi per un momento l'esclusione `ip dhcp excluded-address 192.168.5.129 192.168.5.200` e collega un quarto PC in DHCP.
3. Perché un tecnico potrebbe voler limitare il numero di dispositivi, invece di lasciare libero tutto il pool?

---

[← Esercizio precedente](06-lan-nuova-filiale.md) · [Soluzione](../soluzioni/07-router-di-casa.md) · [Esercizio successivo →](08-rete-per-reparti.md)
