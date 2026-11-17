# paithon MCP

[English](README.en.md) · Italiano

Il server MCP pubblico di [paithon.it](https://paithon.it), il magazine italiano sull'intelligenza artificiale. Dà a un assistente AI (Claude, Cursor, Codex e gli altri client che parlano MCP) accesso in sola lettura a quello che il sito mostra a un visitatore anonimo: articoli, dossier, riletture di paper, [Paithon Book](https://book.paithon.it/main/), la pagina sull'AI Act e i numeri di Italia 2070. Non serve un account e non serve una chiave.

Indirizzo: `https://paithon.it/mcp` (Streamable HTTP, senza autenticazione).

## A che cosa serve

Ogni strumento risponde a una domanda che si può fare davvero.

| Strumento | Una domanda che gli si può fare |
|---|---|
| `cerca` | «Che cosa ha scritto paithon sul RAG?» |
| `leggi` | «Leggimi l'articolo su questo indirizzo e riassumilo.» |
| `raccolta` | «Quali dossier ci sono, e in che ordine conviene leggere quello sugli agenti?» |
| `capitoli` | «Com'è fatto l'indice di Paithon Book?» |
| `rimando` | «Dove spiega il libro l'attention?» |
| `sezione` | «Spiegami la discesa del gradiente come la racconta il libro, al livello elementare.» |
| `paper` | «Che cosa ha paithon sul paper arXiv 1706.03762?» |
| `settimana` | «Quali paper c'erano nella Rassegna dell'ultima settimana?» |
| `mappa` | «Com'è organizzato il sito, e da dove comincio a cercare?» |
| `orienta` | «Voglio capire gli agenti: che cosa devo leggere nel libro, e che cosa sul sito?» |
| `ai_act` | «Che cosa deve fare, secondo l'AI Act, chi mette un chatbot sul proprio sito?» |
| `percorso` | «Programmo già in Python e voglio capire gli LLM: che percorso mi consigli?» |
| `dati_italia` | «Quante imprese italiane usano l'AI, e da quale fonte?» |

## Come si installa

Il server non chiede né un account né una chiave: dove un client propone un accesso (per esempio OAuth), si lasciano vuoti i campi facoltativi di autenticazione. Il servizio è attivo dal 15 novembre 2026. La riga per Claude Code è provata con un client vero; per gli altri client vale la loro pagina ufficiale, citata in fondo.

### Claude Code

```
claude mcp add --transport http paithon https://paithon.it/mcp
```

### Claude Desktop e claude.ai

Un server remoto non si aggiunge nel file `claude_desktop_config.json` (quello è per i server locali): si aggiunge come connettore personalizzato dall'interfaccia. In Personalizza, Connettori, «Aggiungi», «Aggiungi connettore personalizzato», si inserisce il nome `paithon` e l'indirizzo `https://paithon.it/mcp`, e si lasciano vuoti i campi facoltativi di autenticazione. Secondo la documentazione di Claude i connettori personalizzati sono disponibili nei piani Free (uno solo), Pro, Max, Ultra, Team ed Enterprise; nei piani Team ed Enterprise lo aggiunge prima un proprietario dell'organizzazione. Le voci dei menu possono cambiare nome: vale la pagina ufficiale citata in fondo.

### Cursor

In `~/.cursor/mcp.json` (per tutti i progetti) oppure in `.cursor/mcp.json` (per uno solo):

```json
{
  "mcpServers": {
    "paithon": {
      "url": "https://paithon.it/mcp"
    }
  }
}
```

### Codex

Da riga di comando:

```
codex mcp add paithon --url https://paithon.it/mcp
```

oppure in `~/.codex/config.toml`:

```toml
[mcp_servers.paithon]
url = "https://paithon.it/mcp"
```

## Gli strumenti

| Strumento | Argomenti | Che cosa dà |
|---|---|---|
| `cerca` | `testo` (almeno 2 caratteri), `area`, `formato`, `limite` (da 1 a 20, 10 se manca) | Una riga per articolo pubblicato: titolo, link, estratto |
| `leggi` | `url` (indirizzo su paithon.it o slug), `parte` (da 1, 1 se manca) | Il testo dell'articolo, con la citazione da usare. Un articolo oltre i 40.000 caratteri arriva a parti: ognuna dice quante sono e come chiedere la successiva |
| `raccolta` | `slug` (facoltativo) | Senza argomenti, i dossier pubblici e le aree; con lo slug, gli articoli nell'ordine di lettura |
| `capitoli` | nessuno | L'indice di Paithon Book: parti, capitoli, sezioni, con i link |
| `rimando` | `concetto` | I capitoli e le sezioni del libro che lo spiegano; se il libro non ne parla, lo dice |
| `sezione` | `url` (l'indirizzo di una sezione, come lo danno `capitoli` e `rimando`, o il suo titolo), `livello` (`elementare` o `superiore`, facoltativo), `parte` | Il testo di una sezione di Paithon Book, con le formule in LaTeX e il codice. Senza livello ci sono tutti e due, dichiarati; una sezione lunga arriva a parti |
| `paper` | `arxiv` (identificativo o indirizzo di arxiv.org) | La scheda di un paper: abstract, licenza, punti chiave, la nostra lettura, paper vicini. Solo quando la sezione paper del sito è aperta |
| `settimana` | `indietro` (da 0 a 12, 0 se manca) | I paper della Rassegna più recente, o delle settimane prima. Solo quando la sezione paper è aperta |
| `mappa` | nessuno | L'organizzazione del sito: pagine guida, formati, aree con il numero di articoli, dossier |
| `orienta` | `argomento` | Per un argomento, che cosa ne dicono il libro e il sito, divisi per «capire», «fare», «che cosa succede», «approfondire», con un punto di partenza |
| `ai_act` | `tema` (facoltativo) | Che cosa dice oggi la pagina sull'AI Act e sulle leggi italiane, con la data di aggiornamento e le fonti primarie |
| `percorso` | `profilo` (`da-zero`, `programmo`, `llm`, `insegno`, `decido`) oppure `obiettivo` | Le tappe di lettura per profilo o per obiettivo; senza argomenti elenca i cinque profili |
| `dati_italia` | `tema` (facoltativo) | I numeri di Italia 2070 come stanno nella pagina, con la fonte primaria del capitolo |

## Le regole

- **Solo quello che è pubblicato.** Niente bozze, niente contenuti nascosti, niente articoli riservati ai membri oltre l'anteprima che vede chiunque.
- **Ogni risposta porta i link**, e il server chiede al client di citarli. Il libro si cita con il titolo del capitolo e il suo indirizzo, e il suo testo (licenza CC BY-NC-ND 4.0) serve a spiegare: i passi testuali si riportano con la fonte, e il libro non si ridistribuisce dentro un altro prodotto né in versioni modificate.
- **Il libro si legge solo dal suo indice**: `sezione` scarica una pagina di book.paithon.it solo se è fra quelle dell'indice pubblico del libro.
- **Gli articoli che hanno chiesto di restare fuori dalle IA** si trovano con `cerca`, ma `leggi` ne dà solo titolo, estratto e link.
- **Limiti**: 120 chiamate ogni 10 minuti per gruppo di indirizzi; una richiesta al massimo di 65.536 byte; un solo messaggio per volta; `leggi` dà gli articoli lunghi a parti da 40.000 caratteri, così nessuna risposta supera il limite dei client; `cerca` dà al massimo 20 risultati.

## Che cosa non fa

- **Non è consulenza legale.** `ai_act` è un orientamento con le fonti: dice che cosa riporta la pagina del sito e dove stanno i testi di legge, e non sostituisce un parere.
- **Non dà il testo integrale dei paper.** Ci sono i metadati, l'abstract, i punti chiave e la nostra lettura; il testo sta su arXiv.
- **Non fa previsioni.** `dati_italia` riporta i numeri della pagina Italia 2070 e le loro fonti, senza ricalcoli e senza stime o probabilità di paithon.
- **Non scrive e non modifica niente** sul sito: tutti gli strumenti sono di sola lettura.
- **Non sostituisce il libro**: Paithon Book spiega come funziona una cosa e si legge in ordine; gli strumenti ti ci portano, e `sezione` ne dà il testo da citare.

## Privacy

Non si salvano le domande né le risposte, e non si tiene traccia di chi chiama. Si conta soltanto quante volte al giorno viene chiamato ogni strumento, in forma aggregata, per sapere quali servono. L'indirizzo di rete serve solo per il limite di chiamate, come impronta firmata che non permette di risalire all'indirizzo. Il resto è nell'informativa su [paithon.it/privacy](https://paithon.it/privacy). Per contattare paithon, il sito pubblica i riferimenti.

## Licenza

Questa documentazione è rilasciata con licenza [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0); il testo ufficiale è nel file `LICENSE`. La licenza copre la documentazione: il codice del server non sta in questo repository. I contenuti che il server restituisce restano soggetti alle licenze del sito e di Paithon Book (CC BY-NC-ND 4.0).

<!-- Pagine consultate il 2026-11-13 per la sintassi dei client:
- Claude Code: https://code.claude.com/docs/en/mcp
- Claude Desktop e claude.ai: https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp
- Cursor: https://cursor.com/docs/context/mcp
- Codex: https://developers.openai.com/codex/mcp (rimanda a https://learn.chatgpt.com/docs/extend/mcp?surface=cli)
- Licenza: https://creativecommons.org/licenses/by/4.0/legalcode.txt
-->
