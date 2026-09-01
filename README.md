# Page Sagra del Fungo Porcino

Landing page QR ordini sagra del fungo porcino rocca priora

Web app mobile-first per la **32ª Sagra del Fungo Porcino** — Colle di Fuori, Rocca Priora (RM).
Si apre scansionando un QR code al tavolo: locandina, poi scelta dello stand gastronomico.

**Live:** https://pasqcu.github.io/page-sagra-del-fungo-porcino/
**Sito ufficiale:** https://www.sagradelfungoporcino.com/

## Struttura

```
index.html                          tutta l'app (HTML + CSS + JS inline)
assets/locandina-sagra.webp         locandina 1200px — 78 KB (anche versione ingrandita)
assets/locandina-sagra-800.webp     locandina 800px  — 44 KB
assets/logo-<nome>.webp             logo nella card, 192px
assets/logo-<nome>-512.webp         logo ingrandito, 512px
```

Due schermate gestite in JS senza ricaricare la pagina:

1. **Benvenuto** — locandina, date, bottone `Ordina qui`
2. **Selezione ristoranti** — 3 card fisse (Love Truffles, Zi Righetto, Premiata Trattoria Prati)
   con logo, nome e link al menu (`target="_blank"`), tasto Indietro

Il router usa l'hash (`#ordina`), così il tasto indietro fisico di Android torna alla locandina
invece di chiudere la pagina.

**Lightbox.** Locandina e loghi si aprono a schermo pieno al tocco; un secondo tocco
sull'immagine ingrandisce ancora e la rende trascinabile. Si chiude con la X, toccando fuori,
con Esc o col tasto indietro — anche la foto aperta occupa una voce di cronologia, quindi
il gesto indietro chiude prima la foto e solo dopo cambia schermata.
Le versioni grandi vengono scaricate al primo tocco, non al caricamento della pagina.

## Scelte tecniche

- **Zero dipendenze esterne.** Niente Tailwind CDN, niente Google Fonts, niente JS di libreria:
  su rete satura ogni richiesta in più è un rischio. Font di sistema, CSS inline, un solo file HTML.
- **WebP a due risoluzioni** via `srcset`, con `width`/`height` e `aspect-ratio` per non avere
  salti di layout durante il caricamento.
- **La locandina si dimensiona sullo spazio che avanza** (`calc(100dvh - 420px)`), così il
  bottone `Ordina qui` resta sopra la piega da 320px fino ai Pro Max, senza scrollare.
- **Peso a freddo: ~50 KB in 2 richieste** (6 KB di HTML + 44 KB di locandina).
- **Touch-friendly**: bottoni ≥ 56px, nessuno scroll orizzontale, `safe-area-inset` per i notch.
- **Animazioni** disattivate automaticamente con `prefers-reduced-motion`.

## Menu: link corti dei QR dinamici

I menu stanno dietro QR dinamici. Nel bottone va il **link corto del QR**, non l'URL finale:
il link corto resta lo stesso per sempre, cambia solo la destinazione a cui punta. Così chi
inquadra il QR fisico e chi tocca il bottone finiscono nello stesso posto, e con un telefono solo.

| Ristorante | Link corto | Destinazione |
|---|---|---|
| Love Truffles | `https://qromo.it/q/?t=zBlAxVZP` | `lovetruffles.qromo.it` |
| Zi Righetto | `https://qromo.it/q/?t=0xG5N3Ff` | `zirighetto.qromo.it` |
| Premiata Trattoria Prati | `https://qromo.it/q/?t=6XtbK94G` | `premiatatrattoriaprati.qromo.it` |

## Loghi

`assets/logo-love-truffles.webp`, `logo-zi-righetto.webp`, `logo-trattoria-prati.webp` —
quadrati 192x192, ~18 KB in totale, precaricati quando il telefono è fermo.
Le varianti `-512` servono solo alla lightbox e si scaricano a richiesta.
Love Truffles e Zi Righetto arrivavano da foto su carta: fondo riportato a bianco pieno,
rumore e ombre rimossi, ritaglio sul disegno. Prati era già pulito, solo ritagliato e ridimensionato.
Tutti e tre sono centrati sul baricentro dell'inchiostro, non sul rettangolo che li contiene:
con code sottili da un lato i due centri non coincidono e il logo sembra storto nella tessera.

## Modifiche

Modifica `index.html`, committa su `main`. Il push aggiorna GitHub Pages in circa un minuto.
