# Genera — gestione accessi

Applicazione web per gestire gli accessi al portale **Genera**: i rivenditori attivati,
con codice, user, password ed email di riferimento.

**👉 https://leliosilipigni.github.io/genera-accessi/**

A cura di **Ellesse Rappresentanze** (Letterio Silipigni).

---

## Come sono protette le credenziali

Questo repository — e la pagina pubblicata — **non contengono nessuna credenziale**.
Verificabile: apri il sorgente e cerca.

Le credenziali vivono **solo nel browser del tuo dispositivo**, dentro `localStorage`,
cifrate con **AES-GCM 256**. La chiave è derivata dalla tua parola d'ordine con
**PBKDF2-SHA256, 250.000 giri** e un sale casuale di 16 byte.

In pratica:

- la parola d'ordine **non viene mai inviata da nessuna parte** e non viene salvata;
- nel browser non c'è mai nulla in chiaro — nemmeno i nomi dei clienti;
- se perdi la parola d'ordine l'archivio **non è recuperabile**: l'unica strada è
  ricominciare e reimportare i dati;
- l'archivio **si blocca da solo dopo 10 minuti** di inattività;
- quando copi una password, gli appunti vengono **svuotati dopo 45 secondi**.

## Uso

**Primo avvio** — scegli una parola d'ordine (almeno 8 caratteri). Da quel momento
l'archivio è cifrato con quella.

**Importare la lista** — dal menu ☰ → *Importa accessi*. Accetta:

- il testo incollato in blocchi da 5 righe: codice, cliente, user, password, email
- file **CSV / TSV** con le stesse colonne
- un **backup .json** cifrato

Se un codice esiste già viene aggiornato, gli altri si aggiungono. Non serve
importare due volte.

**Cercare** — la barra in alto cerca su cliente, codice, user ed email. I chip
filtrano per anno di rotazione della password, oppure mostrano quelle senza anno.

**Copiare** — tocca lo user, la password o l'email: vanno negli appunti. Il pulsante
*Copia tutto* mette insieme i dati della scheda, pronto da incollare in una mail.
L'icona dell'occhio mostra la password: per il resto sono sempre mascherate, e le
email mostrano solo il dominio.

**Rotazione delle password** — il fornitore usa lo schema `<user>-<AA>R!`. Il badge
sulla scheda dice di che anno è: verde se aggiornata, giallo se è dell'anno scorso,
rosso se è di due anni fa o più. Il chip *Da ruotare* le raccoglie tutte. Il pulsante
*Genera* nel form propone la password per l'anno corrente partendo dallo user.

**Backup** — dal menu, *Scarica backup cifrato* produce un file `.json` che contiene
l'archivio **ancora cifrato**: è l'unico file che puoi mettere su Drive o mandarti per
mail senza rischi, perché si riapre solo con la tua parola d'ordine. Conservalo: se
cancelli i dati del browser, è l'unico modo per rientrare.

`Esporta CSV` invece esce **in chiaro**: usalo solo se ti serve per altro, e trattalo
come un foglio con dentro delle password.

## File

| file | cosa è |
|---|---|
| `index.html` | l'applicazione: un file unico, nessuna dipendenza esterna |

Nessun server, nessun database, nessuna chiamata di rete. Funziona anche aperta da
`file://` sul computer, ma **per l'uso normale apri la pagina pubblicata**: i dati
restano legati all'indirizzo da cui apri l'app, quindi un archivio creato da `file://`
non si vede da GitHub Pages e viceversa.

## Un'avvertenza

Questo strumento mette le credenziali al riparo da occhi indiscreti e da un file
dimenticato in giro, ma **non è un gestore di password certificato** e non protegge da
un dispositivo compromesso. Se un giorno questi accessi diventassero più di quanti se
ne gestiscono a mano, la strada è un gestore di password vero (Bitwarden, 1Password),
che ha anche lo sblocco con impronta e la sincronizzazione fra dispositivi.
