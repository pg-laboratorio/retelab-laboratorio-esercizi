# Soluzione – Esercizio 7

**Risultato atteso:** il pool va da 192.168.5.126 a 192.168.5.200, e i tre PC ricevono .126, .127 e .128.

## Il calcolo

L'ultimo indirizzo assegnabile è 126 + 75 − 1 = 200, quindi 192.168.5.200. Il − 1 serve perché il primo indirizzo, .126, è già uno dei 75. È l'errore più frequente: chi lo dimentica scrive .201.

Vanno esclusi due intervalli: da .1 a .125 (sotto il pool) e da .201 a .254 (sopra il pool).

## Configurazione

```
enable
configure terminal
interface g0/0
 ip address 192.168.5.1 255.255.255.0
 no shutdown
exit
ip dhcp excluded-address 192.168.5.1 192.168.5.125
ip dhcp excluded-address 192.168.5.201 192.168.5.254
ip dhcp pool CASA
 network 192.168.5.0 255.255.255.0
 default-router 192.168.5.1
end
```

`show ip dhcp pool` riassume la configurazione:

```
Pool CASA :
 Network            : 192.168.5.0 255.255.255.0
 Default router     : 192.168.5.1
 DNS server         : (nessuno)
 Leased addresses   : 3
Excluded addresses : 192.168.5.1 - 192.168.5.125, 192.168.5.201 - 192.168.5.254
```

## Risposte

1. Sì, se le esclusioni sono giuste. Un PC con un indirizzo sotto .126 indica che manca la prima esclusione.
2. Non riceve nessun indirizzo: il pool è esaurito. Nella prova con l'esclusione .129–.200, il quarto PC riceve un indirizzo 169.254.x.x (APIPA) e il terminale lo spiega: «Il pool DHCP … è esaurito». Con quell'indirizzo non comunica con nessuno. Ricordati di togliere l'esclusione di prova: `no ip dhcp excluded-address 192.168.5.129 192.168.5.200`.
3. Per limitare chi può collegarsi, per esempio dove molti vicini vedono la rete, e per riservare il resto degli indirizzi a dispositivi statici come stampanti o telecamere.
