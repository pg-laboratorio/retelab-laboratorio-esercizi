# Soluzione – Esercizio 8

**Risultato atteso:** nella parte A tutti si pingano senza router. Nella parte B si pingano anche tra reparti, passando dal router, e i broadcast restano dentro ciascun reparto.

## Parte A

Sei PC su 192.168.1.11–.16 /24, nessun gateway necessario, tre switch in catena. I cavi tra gli switch sono marcati «trunk». Tutti i ping riescono perché è un'unica rete.

Osservazione ARP: dopo `arp -d` su PC-Ven1, il ping verso PC-Ven2 fa lampeggiare la richiesta ARP su tutti e tre gli switch e su tutti gli altri cinque PC. **Pacchetti** riporta: «La ricevono: PC-Ven2, PC-Fin1, PC-Fin2, PC-Amm1, PC-Amm2».

## Parte B

```
enable
configure terminal
hostname R1
interface g0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
interface g0/1
 ip address 192.168.2.1 255.255.255.0
 no shutdown
interface g0/2
 ip address 192.168.3.1 255.255.255.0
 no shutdown
end
```

Riconfigura i PC di Finanza e Vendite con i nuovi indirizzi e imposta su **tutti** i PC il gateway del proprio reparto. Attenzione ai PC di Amministrazione: il loro indirizzo non cambia, ma senza gateway comunicano solo tra loro.

Osservazione ARP: dopo `arp -d`, il ping da PC-Ven1 a PC-Ven2 fa lampeggiare la richiesta solo su SW-Vendite, PC-Ven2 e R1. **Pacchetti**: «La ricevono: PC-Ven2, R1».

## Risposte

1. Parte A: la richiesta ARP attraversa i tre switch e arriva a tutti gli altri cinque PC, anche se interessava solo uno. Parte B: arriva solo all'altro PC di Vendite e all'interfaccia del router in quella LAN. Il router non inoltra i broadcast.
2. Parte A: un solo dominio di broadcast. Parte B: tre, uno per interfaccia del router. Gli switch non separano i domini di broadcast, i router sì.
3. Ogni PC genera broadcast (ARP, DHCP e altri). Con 150 PC in un'unica rete, ogni scheda di rete deve ricevere ed esaminare i broadcast di tutti gli altri 149: traffico inutile che cresce con il numero di host.
4. La sicurezza: il router diventa il punto in cui si può decidere chi raggiunge cosa, per esempio impedendo alle Vendite di accedere ai server della Finanza. In ReteLab si può provare con il dispositivo Firewall, che ha regole permit/deny.
