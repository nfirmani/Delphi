# Elettorale


![001](/docs/elettorale/images/ch01/01.001.elettorale.png)

 https://gitpod.io#https://github.com/nfirmani/Delphi/docs/elettorale

 

# Migrazione da applicazione desktop a applicazione RIA

1. **applicazione desktop** 
   - client-server;
   - bde-tcp/ip-dbms;
 
2. **applicazione web - RIA -**  
   - client-server;
   - firedac-tcp/ip-dbms;
   - webserver-http-tls-pagina remota in un browser; 
   - modello asincrono: Eventi e Callback

il framework UNIGUI permette di creare moderne web application ajax-based continuando ad utilizzare Delphi 
(mantenendo la medesima API consente il rendering dell’applicazione all’interno di una pagina remota in un browser).

**Passi per la migrazione dell'applicazione desktop modulo ELETTORALE:**

1) configurazione dbsys, db-applicativo
2) creazione struttura progetto 
3) BDE       --> Firedac
4) vcl-forms --> unigui-forms 

(attenzione a differenza delle classiche applicazioni desktop, 
unigui deve gestire sessioni utente e moduli condivisi tra le sessioni utente. 
Quindi non solo è una buona pratica ma è fondamntale evitare variabili globali)





# Roadmap

...