# Soluzione – Esercizio 6

**Risultato atteso:** i PC ricevono indirizzi da 192.168.1.10 in su, la stampante resta su .100, tutti i ping riescono.

## Configurazione del router

```
enable
configure terminal
hostname R1
interface g0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
exit
ip dhcp excluded-address 192.168.1.1 192.168.1.9
ip dhcp excluded-address 192.168.1.100
ip dhcp pool FILIALE
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
end
```

Stampante statica: 192.168.1.100, 255.255.255.0, gateway 192.168.1.1. PC in DHCP. Verifica: ogni PC pinga 192.168.1.100 e 192.168.1.1; `show ip dhcp binding` elenca solo i due PC, perché la stampante non ha chiesto nulla al DHCP.

## Risposte

1. Tipicamente 192.168.1.10 e 192.168.1.11. L'indirizzo identifica il singolo host e deve essere unico; mask e gateway descrivono la rete, che è la stessa per tutti.
2. Per stampare dalla stessa LAN non serve. Se la si vuole raggiungere da un'altra rete, il gateway è 192.168.1.1: lo stesso che il DHCP ha dato ai PC, visibile con `ipconfig` su un PC.
3. Senza l'esclusione, quando il pool arriva a .100 il DHCP lo assegna a un PC. Nasce un conflitto: PC e stampante con lo stesso IP, e la stampa funziona a intermittenza. Con pochi PC il guasto non si vede subito, ed è per questo che è insidioso.
4. Perché gli altri dispositivi li raggiungono tramite il loro indirizzo: un indirizzo che cambia rompe le configurazioni dei client.

## La prova facoltativa

Con le esclusioni ridotte a .1–.99, `ipconfig /renew` su Admin dà proprio 192.168.1.100: il DHCP non sa che la stampante lo usa già, perché la stampante ha un indirizzo statico. `ipconfig` su Admin e sulla stampante mostra «192.168.1.100(Duplicato)». Il ping da Manager verso .100 risponde comunque, ma non si sa chi risponde: dopo qualche ping `arp -a` su Manager mostra per 192.168.1.100 a volte il MAC della stampante e a volte quello di Admin. Lo stesso si vede con il pulsante **Pacchetti**, passando il mouse sul MAC di destinazione. È la «stampa a intermittenza» della risposta 3.
