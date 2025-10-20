---
creazione: <%tp.date.now()%>
scadenza:
tags:
  - project
stato: <%await tp.system.suggester(
    ["Non Attivo", "Attivo", "Completato"], 
    ["Non Attivo", "Attivo", "Completato"]
, true, "Stato")%>
priorità: <%await tp.system.suggester(
    ["Alta", "Media", "Bassa"], 
    ["Alta", "Media", "Bassa"]
, true, "Priorità")%>
---

# 📝 Note




# 🧠 Brainstorm

- 


## ✅ TODO

- [ ] 


