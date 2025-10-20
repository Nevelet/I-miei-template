<%* 
let URL = await tp.system.prompt("URL")
-%>
---
creazione: <%await tp.date.now()%>
tags:
  - fonte/video
stato: <%await tp.system.suggester(
    ["Non Attivo", "Attivo", "Completato"], 
    ["Non Attivo", "Attivo", "Completato"]
, true, "Stato")%>
autore: "[[<%await tp.system.prompt("Autore")%>]]"
categorie:
URL: <%URL%>
---

![video](<%URL%>)




## 📝 Appunti

- 



## 🌟 Highlights 

- 






