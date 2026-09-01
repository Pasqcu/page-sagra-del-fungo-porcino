# Pubblicare la pagina su Aruba

**Pubblicata il 1 settembre 2026 su https://www.sagradelfungoporcino.com/Ordini_sagra2026/**

Controlli fatti sull'indirizzo pubblicato:

| Controllo | Esito |
|---|---|
| Risposta | `200`, TTFB 73 ms |
| HTML compresso | 6,7 KB (da 21,7 KB) |
| SVG compresso | 16 KB (da 38,3 KB) |
| WebP | `image/webp`, tutti raggiungibili |
| Cache Aruba | `HIT` dalla seconda richiesta |
| `Set-Cookie` | assente — WordPress non intercetta la cartella |
| `http://` e senza `www` | `301` verso l'indirizzo giusto |
| Percorso in minuscolo | `404` (il server distingue le maiuscole) |
| Flusso completo | benvenuto → 3 ristoranti nell'ordine giusto → 3 link corretti in nuova scheda → lightbox → tasto indietro |

Il resto di questa guida serve per rifare la pubblicazione o per aggiornare i file.

Il sito gira su WordPress 7.1 su hosting Linux Aruba. Questa pagina però **non è un plugin
né un tema**: è una cartella di file statici che si affianca a WordPress senza toccarlo.
WordPress non la vede nemmeno, e se un giorno rompi il sito questa pagina continua a funzionare.

---

## Prima di iniziare

Ti serve:

- le credenziali FTP del dominio (dal pannello Aruba)
- un client FTP: **FileZilla** (gratis, Windows/Mac) o **Cyberduck** (Mac)
- il pacchetto `Ordini_sagra2026.zip` preparato in questa cartella

Il pacchetto contiene:

```
Ordini_sagra2026/
  index.html      la pagina
  assets/         mascotte (SVG) e loghi dei ristoranti (6 file WebP)
  .htaccess       tipo MIME del WebP + cache + niente riscritture
```

⚠️ `.htaccess` inizia con un punto: molti programmi lo nascondono. In FileZilla si vede
sempre; nel Finder del Mac premi `Cmd + Shift + .` per mostrare i file nascosti.

---

## Passo 1 — Prendi i dati FTP

1. Entra su **admin.aruba.it** con le credenziali del dominio
2. Vai al **pannello di gestione hosting** del dominio `sagradelfungoporcino.com`
3. Cerca la sezione **FTP** (a volte si chiama "Gestione FTP" o "Account FTP")
4. Annota **host**, **nome utente**, **password**

L'host di solito è `ftp.sagradelfungoporcino.com`. Se la password non la ricordi,
da lì puoi reimpostarla.

---

## Passo 2 — Collegati

In FileZilla, in alto:

| Campo | Valore |
|---|---|
| Host | `ftp.sagradelfungoporcino.com` |
| Nome utente | quello del passo 1 |
| Password | quella del passo 1 |
| Porta | `21` |

Premi **Connessione rapida**.

Se Aruba rifiuta la connessione in chiaro, usa `File → Gestore siti → Nuovo sito` e imposta
**Protocollo: FTP** con **Crittografia: richiedi FTP esplicito su TLS**.

---

## Passo 3 — Trova la cartella pubblica

Dopo il login vedi l'albero delle cartelle del server nel riquadro di destra.

**La cartella giusta è quella che contiene `wp-config.php`, `wp-content` e `index.php`.**

Su Aruba a volte ci sei già dopo il login, a volte devi entrare in una cartella col nome del
dominio (tipo `www.sagradelfungoporcino.com`). Non fidarti del nome: fidati del contenuto.
Se non vedi `wp-config.php`, non sei nel posto giusto.

**Non toccare, non spostare e non rinominare nulla di quello che trovi lì.**

---

## Passo 4 — Carica la pagina

Scompatta `Ordini_sagra2026.zip` sul tuo computer: ottieni una cartella `Ordini_sagra2026`.

Poi, in FileZilla, trascina l'intera cartella `Ordini_sagra2026` dal riquadro di sinistra (il tuo
computer) a quello di destra (il server), dentro la cartella trovata al passo 3.

Aspetta che la coda in basso sia vuota e che non ci siano trasferimenti falliti.

**Alternativa senza FTP:** il pannello Aruba ha anche un gestore file via browser. Se lo trovi,
puoi caricare direttamente `Ordini_sagra2026.zip` nella cartella pubblica e usare la funzione di
estrazione. Assicurati poi che il risultato sia `Ordini_sagra2026/index.html` e non `Ordini_sagra2026/Ordini_sagra2026/index.html`.

### La maiuscola conta

Il server è Linux: `Ordini` e `ordini` sono due indirizzi diversi. La cartella deve chiamarsi
esattamente **`Ordini_sagra2026`**, con la O maiuscola: è l'indirizzo pubblicato.
Chi digiterà tutto minuscolo prenderà un 404 — verificato.

---

## Passo 5 — Verifica

Apri **https://www.sagradelfungoporcino.com/Ordini_sagra2026/** dal telefono, non dal computer:
è lì che verrà usata.

Controlla che:

- [ ] si veda il fungo con la corona e il bottone arancione "Ordina qui"
- [ ] "Ordina qui" porti alle tre card con i loghi
- [ ] toccando un logo si apra a schermo pieno
- [ ] i tre bottoni verdi aprano i menu in una nuova scheda
- [ ] il tasto indietro del telefono torni alla schermata iniziale invece di uscire

---

## Se qualcosa non va

**404 pagina non trovata**
- Maiuscola sbagliata: la cartella deve essere `Ordini_sagra2026`, non tutto minuscolo
- Doppia cartella: hai `Ordini_sagra2026/Ordini_sagra2026/index.html`. Sposta i file su di un livello
- Cartella caricata nel posto sbagliato: rileggi il passo 3

**Errore 500**
- Cancella il file `.htaccess` dentro `Ordini_sagra2026` e ricarica la pagina.
  Serve solo a ottimizzare: senza, la pagina funziona lo stesso

**La pagina si vede ma le immagini no**
- La cartella `assets` non è stata caricata, o è finita fuori da `Ordini`.
  Deve stare in `Ordini_sagra2026/assets/`

**Vedi una versione vecchia dopo un aggiornamento**
- Aruba ha una cache davanti al sito (`x-aruba-cache`). Aspetta qualche minuto,
  oppure apri l'indirizzo con qualcosa in fondo: `/Ordini_sagra2026/?v=2`

---

## Passo 6 — Collegare la pagina al sito (opzionale)

Per aggiungerla al menu di WordPress:

1. Entra in `wp-admin`
2. **Aspetto → Menu**
3. **Link personalizzato** → URL `https://www.sagradelfungoporcino.com/Ordini_sagra2026/`,
   testo `Ordina`
4. **Aggiungi al menu**, poi **Salva menu**

Non creare una pagina WordPress con lo stesso nome della cartella: creerebbe confusione con la cartella.
La cartella vera vince comunque, ma tanto vale non avere due cose con lo stesso nome.

---

## Aggiornamenti futuri

Cambia i file in questo repository, poi ricarica via FTP solo quelli modificati
sovrascrivendo i vecchi. Se cambi solo i link dei menu basta risostituire `index.html`.

I link nei bottoni sono i **link corti dei QR dinamici**: se cambi la destinazione dal
pannello Qromo, questa pagina segue da sola e non va ricaricata niente.
