
<%*
let title = await tp.file.title

// Primo livello: macro-categorie
let mainType = await tp.system.suggester(
    ["Nota", "Progetto", "Fonte", "Vuoto"], 
    ["Nota", "Progetto", "Fonte", "Vuoto"]
, true)

// Configurazione con supporto per sottocategorie
let config = {
    "Nota": {
        template: "[[Note (Template)]]",
        folder: "/Inbox/"
    },
    "Progetto": {
        sub: {
			"Standard": {
                template: "[[Progetto (Template)]]",
                folder: "/Progetti/"
            },
            "YouTube": {
                template: "[[Progetto (Template)]]",
                folder: "/Progetti/YouTube/"
            },
            "Clienti": {
                template: "[[Progetto (Template)]]",
                folder: "/Progetti/Clienti/"
            }
        }
    },
    "Fonte": {
        sub: {
            "Articolo": {
                template: "[[Fonte Articolo (Template)]]",
                folder: "/Fonti/Articoli/"
            },
            "Video": {
                template: "[[Fonte Video (Template)]]",
                folder: "/Fonti/Video/"
            },
            "Libro": {
                template: "[[Fonte Libro (Template)]]",
                folder: "/Fonti/Libri/"
            }
        }
    },
    "Vuoto": {
        template: "[[Vuoto (Template)]]",
        folder: "/Inbox/"
    }

}



// Se ha sottocategorie, chiede anche il sotto-tipo
let c
if(config[mainType].sub){
    let subType = await tp.system.suggester(
        Object.keys(config[mainType].sub),
        Object.keys(config[mainType].sub)
    )
    c = config[mainType].sub[subType]
} else {
    c = config[mainType]
}


// Applica template, sposta e rinomina
return await tp.file.include(c.template) + await tp.file.move(c.folder + title)


%>
