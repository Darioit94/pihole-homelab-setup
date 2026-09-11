Readme · MD
Pi-hole Network-wide Ad Blocker (Home + Office Setup)

Setup di un DNS sinkhole basato su Pi-hole per filtrare pubblicità e tracker a livello di rete, con dashboard sempre visibile su schermo dedicato in modalità kiosk.

Obiettivo del progetto

Bloccare pubblicità e tracker per tutti i dispositivi connessi alla rete (non solo browser, anche app), senza installare ad-blocker separati su ogni dispositivo. Progetto pensato anche per funzionare su due sedi diverse (casa/ufficio), spostando fisicamente il Raspberry Pi tra le due reti.
Hardware utilizzato

    Raspberry Pi 4 Model B (2GB RAM)
    Scheda microSD 32GB
    Schermo dedicato per il Pi
    Alimentatore 5V/3A

Stack software

    Raspberry Pi OS (Desktop)
    Pi-hole (DNS sinkhole)
    Firefox in modalità kiosk (dashboard always-on)

Cosa ho fatto

    Flash del sistema operativo su microSD tramite Raspberry Pi Imager, con configurazione avanzata (hostname, utente, Wi-Fi, SSH abilitato) preimpostata prima del primo boot
    Primo avvio e accesso via SSH dal laptop di sviluppo (headless, senza monitor collegato al Pi)
    Installazione di Pi-hole tramite script ufficiale, con configurazione di IP statico e provider DNS upstream
    Configurazione router: DHCP reservation per IP statico del Pi, impostazione DNS primario/secondario a livello di rete
    Blocklist aggiuntive: integrazione di liste di terze parti (es. OISD) per aumentare la copertura del filtro, oltre alla lista di default
    Kiosk mode: configurazione di un file .desktop in autostart per aprire automaticamente la dashboard Pi-hole a schermo intero all'accensione, senza intervento manuale

Problemi incontrati e risolti

    Connessione Wi-Fi non stabilita al primo boot: la configurazione Wi-Fi pre-caricata nell'Imager non si è attivata automaticamente; risolto collegando temporaneamente monitor/tastiera e configurando la rete manualmente dal desktop del Pi
    SSH "Connection refused" nonostante l'opzione fosse abilitata in fase di flash: risolto riabilitando esplicitamente il servizio da terminale (raspi-config / systemctl enable ssh)
    Query DNS non registrate dopo la configurazione: causato dal DNS del router non ancora propagato ai dispositivi già connessi; risolto forzando il rinnovo della configurazione di rete lato client
    Autostart non funzionante per un problema di TERM non riconosciuto durante l'editing via SSH: risolto impostando esplicitamente TERM=xterm per la sessione

Cosa ho imparato

    Differenza tra IP dinamico e IP statico/riservato, e come configurarlo lato router
    Come funziona la risoluzione DNS e perché un DNS sinkhole può filtrare pubblicità a livello di rete (e i suoi limiti, es. su piattaforme come YouTube dove ads e contenuto condividono lo stesso dominio)
    Gestione di servizi Linux con systemctl
    Scansione di rete base con nmap per individuare dispositivi
    Debug di connessioni SSH e problemi di rete partendo dai messaggi di errore
    Struttura e funzionamento dei file .desktop per l'autostart di applicazioni in ambiente Linux

Possibili sviluppi futuri

    Configurazione della stessa infrastruttura su una seconda rete, per uso alternato tra due sedi
    Aggiunta di monitoraggio/alerting per notifiche quando il Pi va offline
    Script Python per analizzare i log delle query DNS

