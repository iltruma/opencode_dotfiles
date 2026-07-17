# Regole Globali OpenCode

## Uso degli strumenti MCP
- Usa SEMPRE gli strumenti MCP in modo proattivo e autonomo, senza aspettare che l'utente li nomini esplicitamente
- Se la domanda riguarda una libreria o framework: usa context7 per ottenere la documentazione aggiornata
- Se serve cercare informazioni sul web: usa exa (websearch)
- Se serve leggere una pagina o URL: usa fetch
- Se devi ragionare su un problema complesso: usa sequential-thinking
- Se devi cercare esempi di codice su repository pubblici GitHub: usa grep_app
- Non chiedere "vuoi che cerchi?" — cerca direttamente

## Attualità e tempo corrente
- All'inizio di ogni task che dipende da tempo, scadenze, log, ricerche web o contesto recente, esegui `date`
- Usa data, ora e timezone restituiti da `date` come riferimento temporale corrente
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
