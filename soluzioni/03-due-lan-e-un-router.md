# Soluzione – Esercizio 3

**Risultato atteso:** tutti i ping riescono. Senza gateway, PC-A1 raggiunge ancora PC-A2 ma non PC-B1.

La rete completa è in [`rete-esercizio-3.json`](../reti/rete-esercizio-3.json).

## Svolgimento

1. Costruisci la LAN A (Switch-A, PC-A1, PC-A2) e la LAN B allo stesso modo.
2. Aggiungi il router e collegalo prima a Switch-A (G0/0) e poi a Switch-B (G0/1). I nomi delle porte compaiono sui cavi.
3. Configura i quattro PC con IP, mask e gateway della tabella.
4. Configura il router con i comandi del testo.
5. Verifica con `show ip interface brief`: G0/0 e G0/1 devono avere l'IP giusto e lo stato up/up. Sullo schema i cavi del router diventano continui, con i pallini verdi.
6. Esegui le tre prove nell'ordine indicato.

## Cosa fa ogni comando

| Comando | Effetto |
| --- | --- |
| `enable` | Entra in modalità privilegiata (prompt `R1#`) |
| `configure terminal` | Entra in modalità di configurazione globale (`R1(config)#`) |
| `hostname R1` | Dà il nome al router |
| `interface g0/0` | Seleziona l'interfaccia da configurare (`R1(config-if)#`) |
| `ip address <ip> <mask>` | Assegna indirizzo e mask all'interfaccia |
| `no shutdown` | Accende l'interfaccia, spenta di default sui router Cisco |
| `exit` | Torna al livello precedente |
| `end` | Torna alla modalità privilegiata |

## Perché le prove sono in quell'ordine

È un metodo di diagnosi: prima la propria LAN, poi l'interfaccia vicina del router, poi quella lontana, infine l'host remoto. Il primo ping che fallisce indica dove cercare il problema.

## Risposte

1. L'output atteso:

   ```
   Interface              IP-Address      OK? Method Status                Protocol
   GigabitEthernet0/0     192.168.10.1    YES manual up                    up
   GigabitEthernet0/1     192.168.20.1    YES manual up                    up
   GigabitEthernet0/2     unassigned      YES unset  administratively down down
   GigabitEthernet0/3     unassigned      YES unset  administratively down down
   ```

   La colonna Status dice se l'interfaccia è accesa e collegata, la colonna Protocol se il collegamento funziona. «up/up» è lo stato corretto; «administratively down» significa che manca `no shutdown`; «down/down» con l'interfaccia accesa significa che manca il cavo. G0/2 e G0/3 non sono usate, quindi restano spente.
2. Senza gateway il ping a PC-B1 fallisce: PC-A1 sa che 192.168.20.10 è in un'altra rete ma non sa a chi consegnare il pacchetto. Il ping a PC-A2 funziona ancora, perché è nella stessa rete e non passa dal router.
3. Il router collega reti diverse e deve far parte di ciascuna per ricevere i pacchetti diretti fuori da quella rete: ogni interfaccia è un «piede» in una rete, con un indirizzo di quella rete. Lo switch lavora dentro una sola rete, con gli indirizzi MAC.
