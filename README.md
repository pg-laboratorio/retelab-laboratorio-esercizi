# ReteLab – Laboratorio di networking

Dieci esercizi pratici di networking da svolgere nel browser con **[ReteLab](https://pg-laboratorio.github.io/retelab/)**, il simulatore di reti del laboratorio. Si parte da due PC collegati a uno switch e si arriva alla rete di un'azienda divisa per reparti.

Gli esercizi mettono in pratica gli argomenti del corso: indirizzi IP e subnet mask, switch e router, gateway predefinito, DHCP, indirizzi MAC e ARP, diagnosi dei guasti. I router si configurano con i comandi in stile Cisco.

## Come iniziare

1. Apri [pg-laboratorio.github.io/retelab](https://pg-laboratorio.github.io/retelab/). Non serve registrarsi né installare nulla.
2. Scegli i dispositivi dall'elenco **Dispositivi**, collegali con lo strumento **Cavo** e configurali cliccandoci sopra.
3. Apri l'esercizio dalla cartella [`esercizi`](esercizi/) e segui le istruzioni.

Alcuni esercizi partono da una rete già pronta, nella cartella [`reti`](reti/). Per usarla: apri il file su GitHub, premi **Download raw file**, poi in ReteLab **Importa → Apri un file .json**.

## Esercizi

| N. | Esercizio | Argomenti | Rete di partenza | Tempo |
| --- | --- | --- | --- | --- |
| 1 | [Due PC e uno switch](esercizi/01-due-pc-e-uno-switch.md) | IP, ping, ruolo dello switch | — | 20 min |
| 2 | [Chi sta nella mia rete?](esercizi/02-chi-sta-nella-mia-rete.md) | Subnet mask, indirizzo di rete | — | 30 min |
| 3 | [Due LAN e un router](esercizi/03-due-lan-e-un-router.md) | Router, gateway predefinito, comandi Cisco | — | 45 min |
| 4 | [Indirizzi automatici con DHCP](esercizi/04-dhcp.md) | DHCP, pool, esclusioni | [rete dell'es. 3](reti/rete-esercizio-3.json) | 40 min |
| 5 | [Trova i guasti](esercizi/05-trova-i-guasti.md) | Troubleshooting | [rete con 5 guasti](reti/05-trova-i-guasti.json) | 45 min |
| 6 | [La LAN della nuova filiale](esercizi/06-lan-nuova-filiale.md) | DHCP + IP statico | — | 45 min |
| 7 | [Configurare il router di casa](esercizi/07-router-di-casa.md) | Calcolo del pool DHCP | — | 40 min |
| 8 | [Dividere la rete per reparti](esercizi/08-rete-per-reparti.md) | Sottoreti, domini di broadcast | [parte A, facoltativa](reti/08-parte-a.json) | 60 min |
| 9 | [Seguire un pacchetto](esercizi/09-seguire-un-pacchetto.md) | MAC, IP, ARP | [rete dell'es. 3](reti/rete-esercizio-3.json) | 50 min |
| 10 | [Caccia all'errore a squadre](esercizi/10-caccia-all-errore.md) | Troubleshooting a squadre | [rete dell'es. 3](reti/rete-esercizio-3.json) | 2 ore |

## Consegna

Per ogni esercizio consegna:

- il file della rete: **Esporta → Scarica file .json**, con il nome `Cognome_EsN` (per esempio `Rossi_Es3`);
- uno screenshot della topologia;
- la tabella degli indirizzi compilata;
- le risposte alle domande.

## Comandi utili

| Dove | Comando | A cosa serve |
| --- | --- | --- |
| PC | `ping <indirizzo>` | Verifica se un host risponde |
| PC | `ipconfig` / `ipconfig /all` | Mostra IP, mask, gateway e (con `/all`) MAC e server DHCP |
| PC | `ipconfig /renew` | Chiede di nuovo l'indirizzo al DHCP |
| PC | `arp -a` / `arp -d` | Mostra / svuota la tabella ARP |
| PC | `tracert <indirizzo>` | Mostra i router attraversati |
| Router | `enable`, `configure terminal` | Entra in modalità privilegiata e poi di configurazione |
| Router | `show ip interface brief` | Interfacce, indirizzi e stato |
| Router | `show running-config` | Configurazione attiva |
| Router | `show ip route` | Tabella di routing |
| Router | `show ip dhcp binding` | Indirizzi assegnati dal DHCP |
| Router | `show arp` | Tabella ARP del router |

`help` nel terminale elenca tutti i comandi disponibili. Dopo un ping, il pulsante **Pacchetti** mostra il viaggio tratto per tratto, con indirizzi MAC e IP.

## Modalità esercizio

La rete dell'esercizio 5, e quelle che le squadre si scambiano nell'esercizio 10, si aprono in **modalità esercizio**: non c'è il pulsante Verifica rete, il terminale non spiega perché un ping fallisce e sotto i PC non compaiono gli indirizzi. I guasti si trovano solo con i comandi. La modalità si attiva da **Esporta** e si disattiva cliccando l'etichetta in alto: durante l'esercizio non va disattivata.

## Struttura della repository

```
retelab-laboratorio-esercizi/
├── README.md
├── esercizi/      testi degli esercizi
├── soluzioni/     soluzioni commentate
├── reti/          reti di partenza da importare in ReteLab
├── LICENSE        licenza MIT (configurazioni, comandi e reti)
└── LICENSE-CC-BY-NC-SA-4.0.md   licenza dei testi
```

## Licenza

- I **testi** degli esercizi e delle soluzioni sono distribuiti con licenza [CC BY-NC-SA 4.0](LICENSE-CC-BY-NC-SA-4.0.md).
- Le **configurazioni, i comandi e i file delle reti** sono distribuiti con licenza [MIT](LICENSE).

Gli esercizi 6–10 sono ispirati alle attività Packet Tracer del corso Cisco *Networking Basics*, ma sono testi originali pensati per ReteLab.
