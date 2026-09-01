# Page Sagra del Fungo Porcino

Landing page QR ordini sagra del fungo porcino rocca priora

Web app mobile-first per la **32ª Sagra del Fungo Porcino** — Colle di Fuori, Rocca Priora (RM).
Si apre scansionando un QR code al tavolo: locandina, poi scelta dello stand gastronomico.

**Live:** https://pasqcu.github.io/page-sagra-del-fungo-porcino/
**Sito ufficiale:** https://www.sagradelfungoporcino.com/

## Struttura

```
index.html                        tutta l'app (HTML + CSS + JS inline)
assets/locandina-sagra.webp       locandina 1200px — 78 KB
assets/locandina-sagra-800.webp   locandina 800px  — 44 KB
```

Due schermate gestite in JS senza ricaricare la pagina:

1. **Benvenuto** — locandina, date, bottone `Ordina qui`
2. **Selezione ristoranti** — 3 card con logo, nome e link al menu (`target="_blank"`), tasto Indietro

Il router usa l'hash (`#ordina`), così il tasto indietro fisico di Android torna alla locandina
invece di chiudere la pagina.

## Scelte tecniche

- **Zero dipendenze esterne.** Niente Tailwind CDN, niente Google Fonts, niente JS di libreria:
  su rete satura ogni richiesta in più è un rischio. Font di sistema, CSS inline, un solo file HTML.
- **WebP a due risoluzioni** via `srcset`, con `width`/`height` e `aspect-ratio` per non avere
  salti di layout durante il caricamento.
- **Touch-friendly**: bottoni ≥ 56px, nessuno scroll orizzontale, `safe-area-inset` per i notch.
- **Animazioni** disattivate automaticamente con `prefers-reduced-motion`.

## Cosa resta da inserire

Tutti i punti sono marcati con commenti espliciti dentro `index.html`:

| Cosa | Dove |
|---|---|
| Loghi ristoranti | `assets/logo-ristorante-1.webp`, `-2`, `-3` — finché mancano la card mostra un segnaposto |
| Nomi ristoranti | `<h3 class="card-name">` in ogni card |
| Link ai menu | `href="#"` nei bottoni `Consulta il Menu e Ordina` |

## Modifiche

Modifica `index.html`, committa su `main`. Il push aggiorna GitHub Pages in circa un minuto.
