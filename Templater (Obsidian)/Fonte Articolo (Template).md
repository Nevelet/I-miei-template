---
creazione: <%tp.date.now()%>
tags:
  - fonte/articolo
stato: <%await tp.system.suggester(
    ["Non Attivo", "Attivo", "Completato"], 
    ["Non Attivo", "Attivo", "Completato"]
, true, "Stato")%>
categorie:
URL: <%await tp.system.prompt("URL")%>
autore: "[[<%await tp.system.prompt("Autore")%>]]"
---

## 📝 Appunti

- 



## 🌟 Highlights 

- 


