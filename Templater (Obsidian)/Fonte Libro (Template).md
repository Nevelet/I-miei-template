---
creazione: <%tp.date.now()%>
tags: 
  - fonte/libro
autore: "[[<%await tp.system.prompt("Autore")%>]]"
anno: <%await tp.system.prompt("Anno")%>
pagine: <%await tp.system.prompt("Pagine totali")%>
pagine lette:
progresso:
rating:
stato: <%await tp.system.suggester(
    ["Non Attivo", "Attivo", "Completato"], 
    ["Non Attivo", "Attivo", "Completato"]
, true, "Stato")%>
URL: 
cover: <%await tp.system.prompt("Cover")%>
acquistato:
---

## 📝 Appunti

- 



## 🌟 Highlights 

- 






