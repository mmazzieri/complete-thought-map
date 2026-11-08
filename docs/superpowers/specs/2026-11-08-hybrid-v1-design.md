# complete-thought-map 1.0 — il programma ibrido «alla lettera»

Data: 2026-11-08. Stato: bozza da approvare da Mauro.

Fonte: Lion Kimbro, *How to Make a Complete Map of Every Thought You Think*
(2003), CC BY-SA 1.0, copia integrale in `book/`. I numeri di riga `Lnnnn`
si riferiscono a `book/book.txt`. Estratto completo delle specifiche del libro,
con citazioni: [`docs/book-spec-extract.md`](../../book-spec-extract.md), sotto
CC BY-SA 1.0.

Convenzione: **[Libro Lnnnn]** = deciso dal libro; **[Mauro]** = deciso da
Mauro; **[Claude]** = scelta di Claude dove il libro tace, con la ragione.

## 1. Intento

Mauro vuole realizzare il software che Kimbro descrive nel capitolo VI, *The
Question of Computers*, esattamente com'è descritto [Mauro]. È la versione 1.0
del progetto: resta conservata e scaricabile così com'era concepita, anche
quando Mauro farà poi una propria versione attualizzata (carta dove serve,
template stampabili, app universale) nello stesso repository dopo il tag 1.0
[Mauro].

Il perché, nelle parole di Kimbro: le parti più faticose del sistema di quaderni
sono mantenere i TOC e trascrivere gli speed, circa il 90% della manutenzione, e
i computer le fanno bene [Libro L6136-6146]. Di giorno si scrive su carta (i
*pan-subject speeds*), la sera si trascrive in ASCII, si lancia il programma, e
«it spits out updated pages over your printer»; le pagine si forano e vanno nel
raccoglitore [Libro L6151-6166].

## 2. Obiettivi

1. Un programma a riga di comando che realizza ogni voce del Geek Block
   [Libro L6555-6645] (tabella di tracciabilità, §9).
2. Input per file: le trascrizioni serali e le richieste di modifica, compreso
   l'esempio di Kimbro di spostamento di speed tra soggetti, che deve essere
   accettato **parola per parola** [Libro L6174-6184].
3. Database astratto dietro un'interfaccia, implementato in YAML [Libro L6608-6616].
4. Uscita «dirt simple»: .txt e .doc, «2 columns, point 8 text, tabbed, fit 70
   to the page» [Libro L6626-6628]; solo le pagine cambiate dall'ultima
   richiesta, oppure i duplicati richiesti; una ricevuta a ogni esecuzione
   [Libro L6595-6600].
5. Elenchi gestiti: speed list per soggetto, TOC dei titoli POI, elenco REF —
   i tre che Kimbro nomina [Libro L6212-6218].
6. Repository pubblico con libro integrale, documentazione, release 1.0 con
   eseguibili scaricabili, pagina su mauromazzieri.it [Mauro].

## 3. Non obiettivi

- Mappe (SMOC, GSMOC) e qualunque grafica: per Kimbro sono il *secondo*
  programma [Libro L6338-6342].
- GUI: il Geek Block ammette «GUI/CLI» [Libro L6582]; si fa solo la CLI [Claude:
  è l'alternativa più semplice che il libro consente].
- Ottimizzazione dello spazio sulla pagina e reflow: Kimbro dice esplicitamente
  di non farli [Libro L6629-6636].
- Ricerca, sincronizzazione, multiutente, cloud, app: appartengono alla versione
  attualizzata di Mauro, non alla 1.0.
- Elenchi oltre speed, POI e REF (RS, PJ, indice, A/S stampati, DD, CEP, TD):
  il libro non li chiede al programma.

**Anti-obiettivi** (renderebbero la 1.0 un fallimento anche se funziona):
aggiungere funzioni «moderne» che tradiscono il testo; cambiare il flusso
carta → trascrizione → stampa; rinumerare speed già stampati.

## 4. Vincoli

- Linguaggio **C#/.NET 10** [Claude: Kimbro indica «C# or Python» L6204/L6559;
  C# è lo stack di Mauro; .NET 10 è l'SDK installato].
- Licenze: codice **GPL-3.0** [Mauro]; libro e documentazione che ne riprende
  ampie parti **CC BY-SA 1.0** con attribuzione a Lion Kimbro [Libro, licenza].
- Nessuna dipendenza da Microsoft Word installato: il .doc si genera come
  `.docx` con OpenXML (`DocumentFormat.OpenXml`) [Mauro, confermato].
- YAML con `YamlDotNet` [Claude].
- Codice, messaggi del programma e documentazione tecnica in inglese; README con
  sezione italiana; pagina del sito in italiano [Mauro, confermato].
- Il programma gira su Windows, Linux e macOS (eseguibili self-contained).

## 5. Il modello dei dati

### 5.1 Soggetti (GSR + A/S)

Ogni soggetto ha: abbreviazione (chiave, confronto senza distinzione di
maiuscole, unica), nome esteso, stato (`active` | `old`), data di creazione
[Libro L7014-7017, L6974-6979]. Ordinamento sempre per **nome esteso**, mai per
abbreviazione [Libro L1544-1548]. Sulle pagine il soggetto si scrive per
esteso [Libro L1547-1548].

Due soggetti riservati, creati da `init`:
- `UNPLACED` («Unplaced»): riceve gli speed trascritti senza soggetto [Libro
  L1020-1026 ammette righe senza soggetto; L5159-5164 «unplacable list»]. A
  differenza delle pagine Unplaced cartacee (L4400-4401) i suoi speed sono
  numerati [Claude: senza numero non si potrebbero poi spostare con il formato
  di Kimbro, che indirizza per numero].
- `CHAOS` («Chaos») [Libro L4324-4333].

Le abbreviazioni non possono coincidere con le parole chiave del formato (§6)
né con i codici di elenco (`S`, `POI`, `P`, `REF`, `R`).

### 5.2 Elenchi e voci

Ogni soggetto ha tre elenchi: `S` (speed), `POI` (TOC dei titoli POI; alias
`P`), `REF` (elenco riferimenti; alias `R`) [Libro L1552-1563, L1576-1578].

Ogni voce ha: numero, stato, data, e i campi del suo elenco.
- Speed: `hint`, `psi` (facoltativo), `content`, `checked` (colonna V) [Libro
  L2895-2980, L940-951]. Psi è testo libero breve (es. `Goal`, `!`) [Claude:
  il libro non dà codici ASCII per i glifi].
- POI: `title` [Libro L1387-1388].
- REF: `citation` [Libro L1502-1504, L7096-7110].

Numerazione: ogni elenco ha un contatore `next`; una voce nuova prende il numero
successivo; i numeri non si riusano e non si rinumerano mai; i buchi vanno bene
[Libro L5095, L5235-5236, L5263, L7132-7133].

Stati di una voce:
- `live`;
- `moved` con destinazione (soggetto, numero): la voce resta dov'era, barrata,
  con il rimando «Ethics S57» [Libro L5236-5239]; la destinazione registra la
  provenienza;
- `retired` (archiviata): la «spunta rossa» [Libro L3061-3062].

Modifiche: `EDIT` sostituisce un campo e conserva il valore precedente nella
storia della voce; `NOTE` aggiunge un'annotazione che si stampa sotto la voce
[Libro L6590 «change/update/annotate»; colori L1995-1997].

### 5.3 Pagine

Pagina = blocco fisso di **70 numeri** di un elenco: la pagina k di `S`
contiene S(70k-69)…S(70k) [Libro L6628 «fit 70 to the page»; Claude: blocchi
fissi per numero rendono stabile l'identità della pagina, così «pagine
cambiate» è ben definito senza reflow, come chiede L6629-6636]. La pagina è
**cambiata** quando una sua voce è aggiunta, modificata, annotata, spostata,
archiviata o spuntata.

ID di pagina come nel libro: `<Nome esteso> S<primo>-S<ultimo>`, dove primo e
ultimo sono i numeri presenti [Libro L1490-1497]; per POI e REF
`<Nome esteso> POI TOC <k>` e `<Nome esteso> REF <k>` [Claude: il libro non
pagina i TOC]. Da riga di comando una pagina si indica come `ABBR S <k>`
oppure `ABBR S <numero di una voce>` con `--entry` [Claude].

### 5.4 Archiviazione di un soggetto

`ARCHIVE ABBR` porta il soggetto a `old`: non accetta più voci nuove (errore),
resta stampabile e consultabile [Libro L4594-4599].

### 5.5 Database (YAML)

Interfaccia `INotebookStore` (carica/salva l'intero quaderno in modo atomico);
implementazione `YamlNotebookStore` [Libro L6608-6616, «pure interfaces, or a
bridge pattern»]. Una cartella-quaderno:

```
notebook.yaml          # versione formato, configurazione di uscita
subjects.yaml          # GSR + A/S
lists/<ABBR>.yaml      # i tre elenchi del soggetto, con contatori e voci
state.yaml             # revisione di ogni pagina e revisione ultima stampata; file applicati (hash)
inbox/                 # file di input in attesa
applied/               # file applicati
out/<timestamp>/       # pagine prodotte
receipts/<timestamp>.txt
```

Scrittura atomica (file temporaneo + rename) e copia `.bak` del file
precedente [Claude]. Il database si può ispezionare e correggere a mano
[Libro L6602-6605 «manual vision/manipulation of internal databases»]; `ctm
check` lo valida.

## 6. Formato dei file di input

File di testo UTF-8. Righe vuote separano i blocchi; `#` a inizio riga è
commento. Un file si applica **tutto o niente**: se una riga è sbagliata, nulla
viene scritto e la ricevuta elenca gli errori con numero di riga [Claude].
Un file già applicato (stesso hash SHA-256) viene rifiutato, così una seconda
esecuzione non duplica speed [Claude].

### 6.1 Trascrizione dei pan-subject speeds

```
PAN 2026-11-08
Eth | lying to kids | Problem | Is a white lie to a child ever OK?
GKI | wiki structure |  | Subject pages should link to their REFs,
+ and REFs should list the pages that cite them.
    |                |  | Remember to call the library.
```

`PAN <data ISO>` apre il blocco; ogni riga è `soggetto | hint | psi | contenuto`
[Libro L940-951: i quattro campi più psi; data della pagina L958-959]. La
spunta di trascrizione è implicita: trascrivere *è* spuntare [Libro L967-969].
Soggetto vuoto → `UNPLACED` (la terza riga dell'esempio è quindi uno speed a
sé, in Unplaced). Hint e psi possono essere vuoti [Libro L1020-1026]. Una riga
che inizia con `+ ` prosegue il contenuto dello speed precedente: uno speed su
più righe ha un solo numero [Libro L2927-2928; Claude: sintassi].
I numeri si assegnano all'arrivo nell'elenco di destinazione, in ordine di file
[Libro L961, L5235].

### 6.2 Spostamento (formato del libro, parola per parola)

```
MTK
S
13 Eth
19 Spt
```

Prima riga: soggetto di origine; seconda: elenco; poi `<numero> <soggetto di
destinazione>` [Libro L6174-6189]. Vale per `S`, `POI`, `REF`. La voce di
destinazione prende il numero successivo dell'elenco omonimo.

### 6.3 Altre richieste (una per riga)

| Riga | Effetto | Fonte |
|---|---|---|
| `NEW Eth Ethics` | nuovo soggetto | [Libro L6593] |
| `ADD Eth S | hint | psi | contenuto` | aggiunge uno speed | [Libro L6589] |
| `ADD Eth POI | titolo` | aggiunge un titolo POI | [Libro L6214-6216] |
| `ADD Eth REF | citazione` | aggiunge un riferimento | [Libro L6216] |
| `EDIT Eth S 12 content | testo` (`hint`, `psi`, `title`, `citation`) | modifica un campo | [Libro L6590] |
| `NOTE Eth S 12 | testo` | annota | [Libro L6590] |
| `CHECK Eth S 12` / `UNCHECK Eth S 12` | colonna V | [Libro L2912-2916] |
| `RETIRE Eth S 12` | archivia la voce | [Libro L6592, L3061] |
| `ARCHIVE Eth` | archivia il soggetto | [Libro L6592, L4594-4599] |
| `START Eth S 57` | il prossimo numero dell'elenco sarà 57 (solo in avanti) | [Claude] |

`START` serve a chi ha già un quaderno di carta: se gli speed di Ethics sono
scritti a mano fino a S56, il programma prosegue da S57 senza collidere con le
pagine esistenti; le pagine scritte a mano restano su carta e non si
ristampano. La prima pagina stampata porta l'ID dei numeri presenti (es.
`Ethics S57-S70`) [Claude: il libro non prevede l'importazione; questo è il
minimo per adottare il programma a metà strada, come fa Kimbro stesso,
L6151-6166].

Le parole chiave sono maiuscole e riservate [Claude: il libro dà solo la
sintassi dello spostamento; queste righe la estendono senza ambiguità].
`CHECK` serve perché una pagina ristampata non perda le spunte V fatte a mano
[Claude].

## 7. Comandi

| Comando | Cosa fa | Fonte |
|---|---|---|
| `ctm init <dir>` | crea il quaderno con UNPLACED e CHAOS | [Claude] |
| `ctm submit <file>…` | copia i file in `inbox/` | input «by file» [Libro L6582] |
| `ctm update [--print]` | applica `inbox/` in ordine di nome, produce le pagine cambiate dall'ultima richiesta, scrive la ricevuta | invocazione [Libro L6599-6600] |
| `ctm pages --changed [--print]` | produce le pagine cambiate dall'ultima richiesta | [Libro L6597] |
| `ctm pages --dup <pagina>… [--print]` | duplica pagine; non tocca lo stato «cambiate» | [Libro L6596] |
| `ctm blank pan|speeds [--count n]` | stampa moduli vuoti (pan 40+ righe, speed list) | [Libro L4293-4295, L936-937] |
| `ctm stats` | statistiche del database | [Libro L6605] |
| `ctm show <ABBR> [S|POI|REF] [n]` | visione del database | [Libro L6603] |
| `ctm check` | valida il database | [Libro L6603] |
| `--verbose` (globale) | informazioni di debug | [Libro L6602] |

Opzione `--dir` (default: cartella corrente) per indicare il quaderno.

`--print` manda i file prodotti alla stampante predefinita: su Windows con il
verbo di shell `print` (serve un programma associato al `.docx`, es. Word o
LibreOffice), su Linux/macOS con `lp`. Se la stampa fallisce, i file restano in
`out/` e la ricevuta lo dice [Libro L6162, «system resources (printer)» nel
diagramma; Claude: stampa diretta dove il sistema la consente, senza
dipendere da Word].

«Dall'ultima richiesta» [Libro L6597] significa dall'ultima volta che le pagine
cambiate sono state **prodotte** in `out/`, con o senza `--print`: dopo
`update`, un `pages --changed` immediato non produce nulla. Se la stampa
fallisce, la ricevuta riporta il comando `ctm pages --dup …` già pronto per
ristamparle [Claude: la richiesta è la produzione delle pagine; la stampa è un
servizio in più].

### Ricevuta

Testo, a schermo e in `receipts/`: data e ora; file applicati o rifiutati con
errori e numero di riga; per soggetto ed elenco le voci aggiunte con il numero
assegnato, spostate, modificate, archiviate; soggetti creati; pagine prodotte
(ID e file); esito della stampa [Libro L6600; formato: Claude].

## 8. Uscita

Configurazione in `notebook.yaml` [Libro L6618-6621]: formati (`docx`, `txt`),
corpo (8), colonne (2), voci per pagina (70). Valori iniziali quelli di Kimbro.

**.docx**: A4 [Claude: Mauro è in Italia; configurabile in `letter`], due
colonne, corpo 8, campi separati da tabulazione nell'ordine della speed page:
`V  #  Hint  @  Content` [Libro L2895-2908]. Intestazione: `(Nome esteso)
S__-S__` a sinistra, data di stampa a destra; piè di pagina: casella archivio
`[ ]`, spuntata `[x]` se tutte le voci della pagina sono spostate o archiviate,
e l'ID ripetuto [Libro L2897-2908, L3061-3064]. Un'esecuzione produce un unico
`.docx` con tutte le pagine, un'interruzione di pagina tra l'una e l'altra
[Claude: un solo file da stampare]. Se 70 voci lunghe non stanno in un foglio,
il testo prosegue sul foglio successivo: accettato, come dice Kimbro [Libro
L6629-6636].

Codice colore del libro applicato al testo [Libro L1987-2020]: voce spostata
barrata con rimando in rosso; spunta di archiviazione in rosso; annotazioni in
verde; numeri in blu; contenuto in nero.

**.txt**: una colonna [Claude: le colonne non hanno senso in testo puro], stessi
campi separati da tabulazione, `[x]` per V, `~testo~ -> Ethics S57` per lo
spostamento, `[R]` per l'archiviazione, annotazioni rientrate con `+ `,
`\f` (form feed) tra le pagine.

## 9. Tracciabilità sul Geek Block

| Voce del libro | Riga | Realizzazione |
|---|---|---|
| input by file (changes) | L6582 | §6, `submit`, `inbox/` |
| input by CLI | L6582 | §7 |
| add entries | L6589 | `PAN`, `ADD` |
| change/update/annotate | L6590 | `EDIT`, `NOTE` |
| move entries | L6591, L6174-6189 | blocco di spostamento §6.2 |
| mark to archive | L6592 | `RETIRE`, `ARCHIVE` |
| new subjects | L6593 | `NEW` |
| request duplicates of pages | L6596 | `pages --dup` |
| request all pages updated since last request | L6597 | `pages --changed`, `update` |
| update from the latest request, return a receipt | L6599-6600 | `update`, ricevuta |
| debug information | L6602 | `--verbose` |
| manual vision/manipulation of databases | L6603 | `show`, YAML editabile, `check` |
| database statistics | L6605 | `stats` |
| abstract database, interfaces | L6608-6611 | `INotebookStore` |
| YAML | L6616 | `YamlNotebookStore` |
| lists, entrees, subjects & abbreviations, output configuration | L6618-6621 | §5 |
| .doc and .txt output | L6577-6579 | §8 |
| 2 columns, 8 pt, tabbed, 70 per page | L6627-6628 | §8 |
| printer | L6162, diagramma | `--print` |
| POI titles, REFs, «if there's a TOC for it» | L6214-6218 | elenchi `POI`, `REF` |

## 10. Qualità e verifica

- Test automatici (xUnit) per parser, numerazione, spostamenti, archiviazione,
  pagine cambiate/duplicati, idempotenza, store YAML, uscita .txt e .docx.
- **Test di accettazione**: l'esempio di Kimbro (MTK → Eth/Spt/Imgn, L6174-6184)
  in un quaderno con MTK di 121 speed: il file del libro, copiato tale e quale,
  si applica e produce le destinazioni attese.
- Esempio completo in `samples/`: un quaderno con le trascrizioni di qualche
  sera, comprese righe del libro come «#43:blahblah:Remember to work on BlahBlah.»
  [Libro L3176] rese nel formato `PAN`.
- CI su GitHub Actions: build e test a ogni PR.

## 11. Documentazione

- `README.md` (inglese + sezione italiana): cos'è, da dove viene, installazione,
  flusso serale in cinque passi, licenze.
- `docs/user-guide.md`: il metodo del libro per la parte che il programma tocca,
  con ampie citazioni, e l'uso dei comandi. Sotto CC BY-SA 1.0 [Mauro: la
  documentazione può copiare anche grandi parti].
- `docs/file-format.md`: riferimento completo del formato §6.
- `docs/book-spec-extract.md`: l'estratto delle specifiche dal libro.
- `NOTICE`: chi ha scritto cosa e sotto quale licenza.

## 12. Rilascio e sito

Sequenza, con i punti dove serve il sì scritto di Mauro:

1. PR `v1-hybrid` → `main` nel repository `complete-thought-map`: codice,
   test, documentazione, workflow. **Mauro la unisce.**
2. Tag `v1.0.0`: un workflow GitHub Actions su tag `v*` pubblica una release
   con eseguibili self-contained single-file zippati per `win-x64`,
   `linux-x64`, `osx-x64`, `osx-arm64`. **Il tag si crea dopo il sì di Mauro.**
3. PR nel repository `mauromazzieri` (dopo `git pull`, la copia locale è
   indietro): scheda nella sezione «Progetti» della homepage e pagina
   `/mappa-pensieri/` in italiano che presenta il progetto, cita il libro con
   licenza e attribuzione, e linka la release `v1.0.0` e il repository. Gli
   eseguibili restano su GitHub. Vanno aggiornati: `deploy.yml` (percorso nei
   trigger e passo di caricamento della nuova cartella), la tabella «Chi
   possiede cosa su Aruba» del README (nessun conflitto con `cerqua/`). Testo
   pubblico: fatti verificabili, niente superlativi né confronti.
   **L'unione pubblica il sito: Mauro la unisce dopo che la release esiste.**

## 13. Lasciato a chi costruisce

Struttura dei progetti .NET, nomi interni, libreria per la CLI
(`System.CommandLine` o parsing a mano), dettagli di impaginazione OpenXML,
messaggi d'errore, organizzazione dei test.
