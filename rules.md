# Regole Globali OpenCode

## Uso degli strumenti MCP
- Usa gli strumenti MCP solo quando aumentano accuratezza, attualità o verificabilità; non usarli per risposte semplici o conoscenza stabile.
- Se la domanda riguarda una libreria o framework e serve sintassi/config aggiornata: usa context7.
- Se serve cercare informazioni attuali sul web: usa exa (websearch).
- Se serve leggere una pagina o URL già identificata: usa fetch.
- Se devi ragionare su un problema davvero complesso: usa sequential-thinking.
- Se servono esempi reali da repository pubblici GitHub: usa grep_app.
- Non chiedere "vuoi che cerchi?" — cerca direttamente

## Attualità e tempo corrente
- Per task temporali usa la data del contesto se già disponibile; esegui `date` solo se manca o serve precisione.
- Se esegui `date`, usa data, ora e timezone restituiti come riferimento temporale corrente.
- Prima di ogni ricerca web o fetch, includi l'anno corrente e, quando utile, la data corrente nella query per evitare informazioni obsolete
- Quando confronti eventi o task, calcola sempre quanto tempo è passato rispetto alla data/ora corrente

## Lingua
- Rispondi sempre in italiano
- Commenti nel codice in inglese
- Nomi variabili/funzioni in inglese

## Stile
- Preferisci diff minimi e modifiche chirurgiche
- Non riscrivere file interi se basta un patch
- Quando modifichi, mostra sempre cosa è cambiato

## Qualità
- Cita le fonti quando fai affermazioni tecniche
- Se non sei sicuro di qualcosa, dillo esplicitamente o ricerca sul web
- Non ti inventare soluzioni, cerca sempre qualcuno che l'ha già fatto
- Preferisci soluzioni semplici a over-engineering

## Contesto
- L'ambiente è WSL su Windows (Debian-based)
- I progetti sono tipicamente in /home/cosimo o /mnt/c
- Stack comune: AWS, Terraform, Java, Python, k8s, Ansible
