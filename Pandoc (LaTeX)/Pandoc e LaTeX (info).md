
## Arguments Enhancing export

### Arguments (PDF)

```latex
-f ${fromFormat} 
% serve per prendere il tipo di formato di partenza. 


--resource-path="${currentDir}" 
--resource-path="${attachmentFolderPath}" 
% Il primo `resource-path` dice di prendere i file (.bib, .tex ecc.) dalla cartella corrente del file che andrai ad esportare, e l'altro invece prenderà i file usando il percorso impostato tramite "Posizione predefinita per i nuovi allegati" nelle impostazioni di Obsidian. Puoi aggiungere altri percorsi sia qui che negli extra arguments.


--lua-filter="${luaDir}/pdf.lua" 
% per usare alcuni filtri usando il linguaggio Lua. I filtri del plugin e che vuoi aggiungere, vanno messi dentro la cartella Lua situata dentro la cartella del plugin `.obsidian/plugins/obsidian-enhancing-export`


${ options.textemplate ? `--resource-path="${pluginDir}/textemplate" --template="${options.textemplate}"` : ` ` } 
% è possibile usare i template literals per le condizioni, come sopra.

-o "${outputPath}" 

-t pdf
```



### Extra arguments (PDF)

```latex
--pdf-engine=pdflatex % Motore per la compilazione LaTeX
--citeproc % Attiva le citazioni
--resource-path="${vaultDir}/Pandoc" % Per usare anche la cartella Pandoc dentro il vault per le risorse (file bib, csl ecc.)
--template "Templates/${metadata.template}.tex" % Usare le proprietà per cambiare i template. 
```

## Variabili d'ambiente (Environment Variables)

Queste qui sotto sono le variabili d'ambiente del plugin Enhancing export. Ho modificato `TEXINPUTS` per usare anche la posizione `Pandoc/Templates` dentro il vault per i template che usano il comando `\input`. Per maggiori informazioni, ti invito a guardare il mio video che uscirà presto [sul mio canale](https://www.youtube.com/@GiambattistaCiancio) e che tratterà meglio questa parte.
```js
HOME="${HOME}"
PATH="${HOME}\AppData\Local\Pandoc;${PATH}"
TEXINPUTS="${pluginDir}/textemplate/;${vaultDir}/Pandoc/Templates/;"
```

## Frontmatter

```yaml

# ==========================
# 📝 METADATI DEL DOCUMENTO
# ==========================
title: La Parmigiana # Titolo documento
subtitle: La storia! # Sotto titolo documento
author: Giambattista Ciancio # Autore del documento 
date: 2025 # Data
abstract: "**Lorem Ipsum** è un testo segnaposto utilizzato nel settore della tipografia e della stampa." 
subject: "Ricette italiane" # Soggetto (incluso nei metadati PDF)

# ==========================
# 📘 STRUTTURA DEL DOCUMENTO
# ==========================
documentclass: book # Puoi usare: book, report, article, letter.
documentclass: extbook # Puoi usare extbook, extarticle, extreport per avere una grandezza del font più grande (max. 20pt).

top-level-division: chapter # (facoltativo) utile se vuoi dividere per capitoli nei libri

# ==========================
# 🌍 LINGUA E LOCALIZZAZIONE
# ==========================
lang: it # Lingua del documento (per sillabazione e localizzazione)
babel-lang: italian # Per tradurre indice, capitoli ecc.

# ==========================
# ✏️ FONT E DIMENSIONI
# ==========================
fontsize: 12pt # Dimensione base del font (da 10pt a 12pt)
classoption: 20pt # Alternativa per impostare la dimensione del font (solo con extclass e max. 20pt)
mainfont: "Times New Roman"
sansfont: "Helvetica"
monofont: "Courier New"

# ==========================
# 📄 PAGINA E MARGINI
# ==========================
papersize: a4 # Formato pagina (es. a4, letter)
geometry: margin=2.5cm # Margini personalizzati (es. top=2cm,bottom=2cm)

# ==========================
# 📚 INDICE E NUMERAZIONE
# ==========================
toc: true # Include l’indice (Table of Contents)
toc-depth: 2 # Livello di profondità dell’indice
toc-title: "Indice" # Titolo personalizzato per l’indice
numbersections: true # Numerazione automatica di capitoli/sezioni

# ==========================
# 🔗 LINK E COLORI
# ==========================
colorlinks: true # Abilita link colorati
linkcolor: blue # Colore dei link (es. blue, black, red)

# ==========================
# 📚 CITAZIONI E BIBLIOGRAFIA
# ==========================
csl: stile.csl # Stile di citazione (formato .csl)
bibliography: bibliografia.bib # File BibTeX con citazioni
# Usa insieme all'opzione Pandoc: --citeproc

# ==========================
# ⚙️ CODICE LATEX PERSONALIZZATO
# ==========================
header-includes:
  # Pacchetti utili
  - \usepackage{graphicx} # Per le immagini
  - \usepackage{hyperref} # Per i collegamenti ipertestuali
  - \usepackage{titlesec} # Per modificare titoli e sezioni

  # Ogni sezione/sottosezione su nuova pagina
  - |
    \newcommand{\sectionbreak}{\clearpage}
    \newcommand{\subsectionbreak}{\clearpage}

```

## Comandi

### 1. Metadati e impostazioni di base

```latex
--metadata title="Titolo del documento" % Imposta il titolo (equivale al campo `title:` nel frontmatter)
--metadata author="Giambattista Ciancio" % Imposta l’autore
--metadata date="2 novembre 2025" % Imposta la data nel PDF
--variable papersize=a4 % Dimensione del foglio (es. `letter`, `a4`, `a5`)
--variable fontsize=12pt % Dimensione del testo principale (⚠️   
--variable lang=it % Lingua del documento
--variable geometry:"margin=2cm" % Margini del documento (puoi specificare anche `top=2cm,left=3cm`)

```

### 2. Template e layout

```latex
--template=mio-template.tex % Usa un template LaTeX personalizzato 
--include-in-header=header.tex % Aggiunge codice LaTeX nel preambolo
--include-before-body=intro.tex % Inserisce un file prima del contenuto 
--include-after-body=footer.tex % Inserisce un file dopo il contenuto  
--toc % Genera un indice dei contenuti   
--toc-depth=2 % Profondità dell’indice 
--number-sections % Numera automaticamente le sezioni

```

### 3. Citazioni e bibliografia

```latex
--citeproc % Attiva il processore di citazioni (necessario se usi `.bib`) 
--bibliography=references.bib % File BibTeX con le fonti
--csl=stile.csl % Stile delle citazioni (APA, MLA, ecc.)

--cls https://raw.githubusercontent.com/citation-style-language/styles/refs/heads/master/elsevier-american-chemical-society.csl % Se usi GitHub, apri il file e clicca su Raw e poi copia l'indirizzo. 

```


### 4. Aspetto e stile

```latex
--variable mainfont="Liberation Serif"
--variable sansfont="DejaVu Sans"
--variable monofont="DejaVu Sans Mono"
% Cambia i font principali (funziona se il template supporta `fontspec` e usi `xelatex` o `lualatex`)

--variable colorlinks=true
--variable linkcolor=blue 
% Colora i link
--variable linestretch=1.2 % Spaziatura tra le righe

```


### 5. Motore di rendering PDF

```latex
--pdf-engine=xelatex % (consigliato) per supporto Unicode e font
--pdf-engine=lualatex % alternativa moderna
--pdf-engine=pdflatex % più compatibile, ma meno flessibile
--pdf-engine-opt=--shell-escape % utile per codice che genera grafici o diagrammi

```

### 6. Varie e avanzate

```latex
-o example.pdf % Nome e formato dell'output finale
--from markdown % formato del file originale che deve essere convertito. 
--highlight-style=pygments % stile per il codice evidenziato (`tango`, `kate`, `monochrome`, ecc.)
--listings % per formattare i blocchi di codice (``python,``js, ecc.)
--top-level-division=chapter (o section) % utile per i libri (capitoli invece di sezioni)
--variable classoption=oneside % opzioni del documento (`oneside`, `twocolumn`, ecc.)
--variable header-includes="\usepackage{tikz}" % inserisce pacchetti direttamente

```


## Separare le sezioni e capitoli

Se nel tuo Markdown hai:
```md
# Capitolo 1
## Sezione 1.1
## Sezione 1.2
# Capitolo 2
```

👉 Con `--top-level-division=section` Pandoc lo converte in:
```latex
\section{Capitolo 1}
\subsection{Sezione 1.1}
\subsection{Sezione 1.2}
\section{Capitolo 2}
```

👉 Con `--top-level-division=chapter` invece:
```latex
\chapter{Capitolo 1}
\section{Sezione 1.1}
\section{Sezione 1.2}
\chapter{Capitolo 2}
```

Devi anche usare una **classe LaTeX** che supporti i capitoli, come `book` o `report`.


