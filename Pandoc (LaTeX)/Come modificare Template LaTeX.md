

## Dove scaricarli

- [pandoc-templates.org](https://pandoc-templates.org/)
- [Templates - Overleaf](https://www.overleaf.com/latex/templates/)


## Creare/modificare un template

**IMPORTANTE!** Se il template è diviso in più file, è importante che il preambolo deve essere nello stesso file principale (di solito chiamato main), perché altrimenti se è separato, non funziona con Pandoc su Obsidian.

### Aggiungere contenuto nota da Obsidian

Questo placeholder `$body$` serve per il plugin Pandoc (o Enhancing Export) di Obsidian per considerare il contenuto della nota attiva quando si usa la funzione di esportazione. 



### Controllare il template tramite Frontmatter

Puoi prendere i valori dal frontmatter (ovvero tramite le proprietà di Obsidian). Ad esempio se usi title nel frontmatter, puoi includerlo nel template in questo modo: `$title$`. Qui c'è un esempio di come è stato usato: `\newcommand{\booktitle}{$titolo$}`

Trovi tutti i comandi per Frontmatter qui: https://github.com/Nevelet/I-miei-template/blob/main/Pandoc%20(LaTeX)/Pandoc%20e%20LaTeX%20(info).md#frontmatter

Assicurati inoltre che questi campi nel frontmatter si trovino anche nel template.  



### Cambiare classe del documento (Document class)

Le `documentclass` standard di LaTeX includono `article` (per documenti brevi), `report` (per documenti più lunghi con capitoli), `book`, `letter` e `slides` (per presentazioni). Queste classi sono predefinite e forniscono strutture base per diversi tipi di documenti. 

- **`article`**: Adatta a documenti brevi che non necessitano di capitoli, come articoli scientifici o rapporti brevi.
- **`report`**: Progettata per documenti più lunghi e complessi, come tesi di laurea o relazioni, che utilizzano capitoli e sezioni.
- **`book`**: Ideale per la creazione di libri, con una struttura che include capitoli, parti e un'impaginazione pensata per la stampa su un solo lato del foglio.
- **`letter`**: Pensata specificamente per la composizione di lettere e include la formattazione tipica di questi documenti.
- **`slides`**: Utilizzata per generare presentazioni, spesso con un layout a una diapositiva per pagina.


Di solito `documentclass` si trova **nelle primissime righe**, e appare così:
```latex
\documentclass[$if(fontsize)$$fontsize$$endif$,$if(papersize)$$papersize$$endif$,$for(classoption)$$classoption$$sep$,$endfor$]{$documentclass$}

```

Esempio frontmatter:
```yaml
documentclass: book
papersize: a5paper
```

Puoi modificare questa riga ad esempio per **forzare sempre** una determinata classe o aggiungere opzioni:
```latex
\documentclass[12pt,a4paper]{report}
```

oppure mantenendo le variabili Pandoc ma aggiungendo qualche tua opzione:
```latex
\documentclass[$if(fontsize)$$fontsize$$endif$,$if(papersize)$$papersize$$endif$,twoside,openright]{$documentclass$}
```


### Cambiare la Lingua

Alcuni comandi di LaTeX come `\tableofcontents` crea una tabella dei contenuti con scritto Contents in inglese, ma se vuoi metterlo in italiano, devi inserire nel preambolo questo pacchetto: `\usepackage[italian]{babel}`

### Aggiungere Link ipertestuali 

Basta usare questo pacchetto. `\usepackage{hyperref}`

Ci sono poi diverse opzioni opzionali che puoi mettere tra le parentesi quadrate. Ecco un esempio: `\usepackage[colorlinks=true, linkcolor=blue]{hyperref}`
In questo modo applica un colore nel link e puoi scegliere volendo anche il colore con `linkcolor`. Se non metti `linkcolor=blue` viene usato il colore predefinito che di solito è rosso. 

Puoi anche abilitarlo nel frontmatter in questo modo:
```latex
$if(hyper)$
\usepackage[colorlinks=true, linkcolor=blue]{hyperref}
$endif$

```

Oppure puoi indicare un'altra azione quando `hyper` non è presente nel frontmatter. Ecco un esempio:
```latex
$if(hyper)$
  \usepackage[colorlinks=true, linkcolor=blue]{hyperref}
$else$
  \usepackage{hyperref}
$endif$
```

Se `hyper` nel frontmatter si trova su `true`, userà il pacchetto `\usepackage{hyperref}` e aggiungerà i colori ai link, altrimenti se `false`, non userà nessun colore. 
```YAML
hyper: true
```

Altra variante per definire i colori tramite il frontmatter.
```latex
$if(hyper)$
  \usepackage[
    colorlinks=true,
    linkcolor=$if(linkcolor)$$linkcolor$$else$blue$endif$
  ]{hyperref}
$else$
  \usepackage{hyperref}
$endif$

```

Esempio frontmatter:
```yaml
hyper: true
linkcolor: red
```


Se ricevi questo errore è perché `\hypersetup` ha bisogno di avere abilitato il pacchetto `\usepackage{hyperref}` e quindi quando non è presente, da l'errore. Se non è necessario, puoi togliere `\hypersetup`
```latex
Error producing PDF.
! Undefined control sequence.
l.83 \hypersetup
```


### Usare le citazioni e immagini in WikiLinks

Per usare le citazioni e immagini in formati WikiLinks devi aggiungere questo placeholder (che trovi sotto) e inserirlo nella sezione sopra `\begin{document}` ovvero nel preambolo del file main.
```latex
$common.latex()$
```

_Questo è stato preso dal template che usa di default Pandoc._

Nota: Per abilitare le citazioni, devi ovviamente aggiungere il comando `--citeproc` negli argomenti di Pandoc. 

## Posizione

I template possono essere copiati sia dentro la cartella di Pandoc nel proprio PC (si trova su Windows qui: `AppData/Roaming/pandoc/templates` e su Mac qui: `~/.local/share/pandoc/templates`) o in qualsiasi altro percorso. 

Se li copi in un'altra cartella, ricordati che devi poi indicare la posizione. Ad esempio cosi: `--template posizione/utente/template.tex`. Se invece il template si trova nella stessa directory di dove avvii i comandi con il prompt, allora non è necessario indicare il percorso completo, puoi fare cosi: `--template template.tex`

### Template suddivisi (\input)

Alcune volte, trovi i template suddivisi: un file principale (di solito chiamato `main.tex`) e gli altri dedicati ad esempio solo al preambolo, all'indice, alla prefazione ecc. Questi file, vengono poi richiamati con il comando `\input` dentro il file principale. Questo `\input{content/01}` infatti serve per prendere un file `.tex` da un'altra cartella, in questo caso dalla cartella `content`. Questo è molto comodo perché puoi spezzettare il codice in più parti e renderlo più gestibile. 

Il file principale può essere messo in qualsiasi posto. Se lo lasci nella posizione `AppData/Roaming/pandoc/templates` non è necessario specificare questo percorso nel prompt, perché viene cercato di default lì. L'unica cosa è che se il template richiama altri file con `\input`, quest'ultimi, devono trovarsi nello stesso percorso di dove hai avviato i comandi. Questo perché utilizza una variabile per gli input con la directory corrente. 

Ad esempio se lasci il template principale dentro la cartella di Pandoc (`AppData/Roaming/pandoc/templates`) inclusi anche i vari file associati e avvii i comandi in questo percorso `desktop/folder`, la funzione `\input` dentro il template main, cercherà i vari file su `desktop/folder`. Quindi in questo caso, dovrai mettere i file in quest'ultimo percorso. 


**Pandoc (o Enhancing Export) su Obsidian**
Se utilizzi Pandoc (o Enhancing Export) su Obsidian, devi indicare negli extra arguments la posizione del template in questo modo: `--template="Pandoc/Templates/template.tex"` 

Ancora una volta, se il template usa altri file con `\input`, allora devi indicarlo nelle Variabili d'ambiente (Environment Variables). Ad esempio in questo modo: `TEXINPUTS="${pluginDir}/textemplate/;${vaultDir}/Pandoc/Templates/;"` 

Nota che nella variabile `TEXINPUTS` (che sta per il formato `.tex`, e input ovvero il comando `\input`), puoi indicare 2 posizioni diverse, come nell'esempio. Per separare i due o più percorsi, bisogna usare il punto e virgola `;`. 

Quindi la variabile `TEXINPUTS` viene usata da LaTeX (non da Pandoc direttamente) per sapere **dove cercare i file `.tex`**, `.cls`, `.sty`, ecc.

Se non vuoi modificare `TEXINPUTS` nelle variabili d'ambiente, puoi mettere i file separati nella cartella corrente del file che vuoi esportare.


#### Metodi posizione template

**1. Usa una cartella del progetto (soluzione semplice e consigliata)**

Metti **main.tex** e le **cartelle associate** (es. `frontmatter`, `chapters`, ecc.) **nella stessa cartella** dove stai lavorando, come:
```cpp
C:\Users\Giambattista\Documenti\Pandoc\Progetto1\
├── main.tex
├── frontmatter\
│   ├── titlepage.tex
│   ├── copyrightpage.tex
│   └── ...
├── file.md

```

E poi:
```bash
pandoc file.md -o output.pdf --template=main.tex
```


**2. Crea un “template globale” con struttura completa**

Se vuoi davvero tenerlo in `AppData\Roaming\pandoc\templates`, allora **devi mantenere la struttura completa dentro quella cartella**, per esempio:
```cpp
C:\Users\<tuo_nome>\AppData\Roaming\pandoc\templates\mio-template\
├── main.tex
├── frontmatter\
│   ├── titlepage.tex
│   ├── preface.tex
│   └── ...

```

Poi, quando lanci Pandoc, devi specificare **il percorso completo del template** (non solo il nome):
```bash
pandoc file.md -o output.pdf --template="C:\Users\<tuo_nome>\AppData\Roaming\pandoc\templates\mio-template\main.tex"

```

⚠️ Ma attenzione: se nel `main.tex` hai riferimenti del tipo `\input{frontmatter/titlepage}`, devono essere **relativi a quel file**, non assoluti — e questa struttura lo consente.


**3. (Opzionale) Impostare una variabile `TEXINPUTS`**

Se vuoi che LaTeX “veda” automaticamente le cartelle aggiuntive ovunque tu sia, puoi impostare la variabile d’ambiente `TEXINPUTS`.
```bash
set TEXINPUTS=C:\Users\<tuo_nome>\AppData\Roaming\pandoc\templates\mio-template\;
pandoc file.md -o output.pdf --template=main.tex
```

Questo dice a LaTeX: “Cerca i file da `\input{}` anche in quella cartella”.
👉 È utile se vuoi mantenere un’unica installazione globale del template.


**Consiglio pratico**
Per evitare mal di testa:
- tieni i template “complessi” (con cartelle) **in una cartella di progetto**,
- usa la cartella `AppData\Roaming\pandoc\templates` **solo per i template singoli** (`.tex` unici).




## Errori Pandoc

**Aggiornamento necessario per alcuni pacchetti**
Se in alcuni template ricevi questo errore: `! Undefined control sequence. <argument> \prop_gput_if_not_in:NnV \g__figureversions_figurestyles_proporti... l.435 ...figureversions_text_str { OsF } { TOsF }` significa che il template richiede una versione di LaTeX troppo recente rispetto a quella installata sul tuo sistema. Il comando `\prop_gput_if_not_in:NnV` fa parte del pacchetto `expl3`, introdotto in LaTeX 2020+. Se la tua distribuzione LaTeX (come TeX Live o MiKTeX) non è aggiornata, non riconosce questo comando e fallisce la compilazione. 

Se usi Se usi MiKTeX: Apri MiKTeX Console → Updates → Check for updates → Update now. Se vedi che ci sono tante cose da aggiornare (nel mio caso la prima volta c'erano 123 aggiornamenti), non preoccuparti, è normale, perché quando lo installi per la prima volta, ti installa la versione base e non ti scarica proprio l'ultimissima versione. Inoltre tutte queste cose da aggiornare richiedono pochi MB. 



