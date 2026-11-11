---
name: section-explainer
description: Fa conoscere all'utente una sezione del software (un modulo, una feature, una cartella, un flusso, un servizio) spiegandone il flusso dall'ingresso all'esito, con codice reale; quando la sezione è grande costruisce una guida interattiva a passi, e alla fine verifica con domande mirate se l'utente ha capito davvero. Usala ogni volta che l'utente dice cose come "spiegami questa parte del codice", "come funziona il modulo X", "fammi capire il flusso di…", "devo lavorare su questa sezione, introducimela", "onboarding su…", "walkthrough di…", "explain this module", "how does X work", oppure richiama questa skill per nome — anche se non la nomina, basta che voglia capire una porzione esistente del software, non modificarla. Vale per qualsiasi linguaggio e framework, e per qualsiasi assistente di programmazione in grado di leggere file.
---

# Section Explainer

Il tuo compito è portare l'utente a **conoscere davvero** una sezione del software:
non solo sapere che esiste, ma saperla seguire da solo — da dove entra una richiesta,
dove passa, dove finisce, e perché è fatta così. Il lavoro ha tre tempi:

1. **Capire tu** la sezione, leggendo il codice.
2. **Spiegarla** all'utente partendo dal flusso, in chat o, se è grande, con una
   guida interattiva.
3. **Verificare** con domande che l'utente abbia capito, e colmare i buchi.

Il terzo tempo non è un optional: una spiegazione che sembra chiara a chi la scrive
spesso non lo è per chi la legge, e l'unico modo per saperlo è chiederglielo.

## Cosa ti serve

Queste istruzioni non dipendono da un modello, da un fornitore o da uno strumento
specifico. Presuppongono solo la capacità di leggere i file del progetto. Tutto il
resto è facoltativo e va usato solo se il tuo ambiente lo offre: ricerca nel
codice, agenti secondari, pubblicazione di pagine web, domande a scelta multipla.
Quando una di queste manca, fai lo stesso lavoro direttamente, più in piccolo.

## Sola lettura

Spiegare non è modificare. Non creare, modificare o cancellare file nel progetto e
non lanciare comandi che hanno effetti collaterali (build, migrazioni, installazioni,
formatter, server). Se un file di appoggio ti serve — la pagina della guida, appunti —
mettilo **fuori dal progetto**, nella cartella scratch/temporanea della sessione.

Se durante la spiegazione l'utente chiede di cambiare qualcosa, è un altro compito:
chiudi o sospendi la spiegazione e chiedigli se vuole passare alla modifica.

## 1. Delimita la sezione

Prima di leggere tutto, chiarisci **cosa** spiegare e **a chi**.

- **Perimetro.** L'utente può nominare una cartella, una classe, una schermata, un
  endpoint, una funzionalità di business ("la fatturazione", "il login"). Traduci la
  richiesta in punti concreti del codice cercando nomi, route, componenti, tabelle.
  Se trovi più candidati plausibili o la sezione è vaghissima ("spiegami il
  backend"), proponi in una riga il perimetro che hai individuato e procedi; chiedi
  solo se le alternative sono davvero diverse tra loro.
- **Scopo.** Perché vuole capirla cambia cosa conta: deve modificarla, correggere un
  bug, rivederla, o è onboarding generale? Se non lo dice, deducilo dal contesto; se
  non puoi, una domanda breve all'inizio vale più di una spiegazione mirata male.
- **Livello.** Adatta il lessico a quello che l'utente mostra di sapere: non spiegare
  cos'è un controller a chi parla di dependency injection, e non dare per scontati
  i concetti del framework a chi dice di non conoscerlo.

## 2. Capisci la sezione

Leggi il codice con un obiettivo preciso: ricostruire il **flusso**.

1. **Trova gli ingressi.** Da dove entra il lavoro nella sezione: route HTTP,
   comando CLI, evento/messaggio, job schedulato, azione dell'utente in UI, API
   pubblica di una libreria. Una sezione può averne più di uno.
2. **Segui chi chiama chi.** Da ogni ingresso, segui le chiamate attraverso gli
   strati (validazione, logica, accesso ai dati, servizi esterni) fino all'esito:
   risposta, scrittura su DB, file, messaggio inviato, cambiamento di UI.
3. **Individua i dati.** Quali strutture viaggiano lungo il flusso e come si
   trasformano; dove vengono lette o salvate.
4. **Segna i bivi.** Condizioni che cambiano il percorso, gestione degli errori,
   permessi, configurazioni e feature flag. Sono quasi sempre le parti che l'utente
   deve capire per lavorarci.
5. **Collega con il resto.** Cosa la sezione usa del resto del sistema e chi, fuori,
   la usa. Basta il confine: non spiegare anche le dipendenze.
6. **Cerca il perché.** Test (dicono il comportamento atteso), commenti, documenti
   di progetto, messaggi di commit (`git log` sui file principali, se disponibile):
   spesso spiegano scelte che il codice da solo non giustifica.

Se la sezione è ampia e l'ambiente offre agenti secondari di sola lettura, delega a
loro l'esplorazione di rami separati e tieni per te le sintesi: ti serve il quadro,
non i file grezzi.

Distingui sempre **fatto osservato** ("questa funzione viene chiamata da X") da
**inferenza** ("probabilmente serve per…"). L'utente costruirà la sua comprensione
su quello che dici: un'inferenza presentata come fatto diventa un errore suo.

## 3. Spiega

### Quanto è grande la sezione

- **Piccola o media** — il flusso sta in una schermata di chat: spiegala in chat.
- **Grande** — esclusi file generati e di configurazione, vale almeno una di queste:
  più di una decina di file rilevanti; più ingressi o flussi distinti; il percorso
  attraversa più strati o più applicazioni (es. frontend, backend e database);
  più di 6–8 passi nel flusso principale. In questo caso costruisci la **guida
  interattiva** (sotto), senza chiedere: è il formato giusto per la richiesta.
  Annuncia in una riga che lo stai facendo e perché.

Le soglie sono indicative: conta se l'utente riuscirebbe a tenere insieme il flusso
scorrendo del testo.

### Spiegazione in chat

Scrivi nella lingua dell'utente, dal generale al particolare:

```
## <Nome della sezione>

### In breve
2-3 frasi: a cosa serve, chi la usa, dove sta nel sistema.

### Flusso
Un diagramma semplice (testo o Mermaid, se l'ambiente lo rende) dall'ingresso
all'esito, poi i passi numerati: per ciascuno cosa succede e dove
(`percorso/file:riga`).

### Pezzi chiave
I file/componenti principali e il ruolo di ciascuno, nell'ordine del flusso.

### Bivi e casi particolari
Errori, permessi, condizioni che cambiano il percorso.

### Da sapere prima di metterci mano
Solo se c'è qualcosa: convenzioni, trappole, accoppiamenti nascosti.
```

Mostra frammenti di codice reale solo dove spiegano meglio delle parole, e tienili
corti. Cita sempre il percorso, così l'utente può aprire il file.

### Guida interattiva (sezione grande)

Se l'ambiente ha uno strumento per pubblicare pagine web o artifact, usalo seguendo
le sue linee guida di design; altrimenti genera un singolo file HTML autonomo, fuori
dal progetto, e di' all'utente dove si trova. Scrivi i testi nella lingua
dell'utente.

La pagina è una **guida a passi**, non un documento da scorrere:

- **Testata compatta**, sempre visibile: nome della sezione, progetto, branch o
  commit letto, numero di file coinvolti.
- **Indice sempre presente** (colonna laterale su schermi larghi, barra in alto su
  telefono), con il passo attivo evidenziato e una spunta sui passi già visti.
- **Un passo alla volta** al centro, con pulsanti precedente/successivo, frecce
  della tastiera e un indirizzo per passo, così da poterlo riaprire o condividere.
- **Panoramica come primo passo**: a cosa serve la sezione e una **mappa del flusso
  cliccabile** — riquadri per i componenti, frecce per le chiamate — in cui ogni
  nodo porta al passo corrispondente. È il cuore della guida: l'utente deve poter
  sempre tornare alla mappa e capire dove si trova.
- **Passi nell'ordine del flusso**, quello in cui una richiesta attraversa il codice.
  Se ci sono più flussi, un gruppo di passi per ciascuno, con la mappa che li
  distingue (colore o selettore).
- **Passo finale "Riepilogo"**: il flusso in una riga per passo, i concetti chiave, e
  un glossario dei termini di dominio e del progetto incontrati.

Dentro ogni passo, blocchi sempre nello stesso ordine e con la propria etichetta, così
l'utente trova le cose nello stesso punto ovunque:

- **Cosa succede**, in due o tre frasi, con la posizione nel flusso (da dove arriva,
  dove va dopo).
- **Codice reale**: snippet presi dal progetto così come sono, con `percorso:riga`.
  Mai codice inventato o "ripulito": l'utente deve ritrovare esattamente quelle
  righe. Evidenzia le righe che contano. Non riportare mai segreti, token o dati
  personali.
- **Dati in gioco**: cosa entra e cosa esce dal passo, se aiuta.
- **Bivi**: condizioni ed errori di quel passo. Dove utile, rendili esplorabili
  (es. un interruttore "e se l'utente non è autenticato?" che mostra il percorso
  alternativo).
- **Da ricordare**: una o due note brevi — perché è fatto così, una trappola, una
  convenzione.

Un passo che non entra in una schermata va diviso, non lasciato crescere.

Opzionale ma utile: un piccolo **"mettiti alla prova"** a fine passo, una domanda
con risposta nascosta da rivelare con un clic. Serve all'utente per autovalutarsi;
non sostituisce la verifica in conversazione.

Dopo aver pubblicato, in chat dai il collegamento e un riassunto di poche righe del
flusso, così l'utente ha il quadro anche senza aprire la pagina, e digli di
scriverti quando l'ha letta (o di farti domande nel frattempo) prima di passare
alla verifica.

## 4. Verifica la comprensione

Quando la spiegazione è consegnata — e, con la guida, quando l'utente dice di averla
letta — verifica che abbia capito. Annuncialo in modo leggero: è un ripasso, non un
esame.

**Come costruire le domande.** Prepara 4–6 domande ancorate al codice reale di
questa sezione, a difficoltà crescente:

1. **Orientamento** — dov'è una cosa, a cosa serve un componente.
   ("In quale file viene validato l'input della richiesta?")
2. **Flusso** — ricostruire una sequenza.
   ("Dopo che l'ordine viene salvato, cosa succede prima che l'utente riceva la
   risposta?")
3. **Bivi** — cosa cambia in un caso particolare.
   ("Cosa succede se il pagamento fallisce dopo la creazione dell'ordine?")
4. **Applicazione** — usare la comprensione per un compito plausibile.
   ("Se dovessi aggiungere un nuovo stato all'ordine, quali punti toccheresti?")

Concentra le domande su ciò che conta per lo **scopo** dell'utente: chi deve
correggere un bug ha bisogno di saper seguire i bivi, chi fa onboarding del quadro
d'insieme. Evita domande di pura memoria (nomi di variabili, numeri di riga) e
domande la cui risposta è suggerita dal testo della domanda.

**Come farle.**

- Una o due alla volta, poi aspetta la risposta. Una raffica di domande diventa un
  modulo da compilare, non un dialogo.
- Preferisci domande aperte; usa la scelta multipla (con lo strumento apposito, se
  l'ambiente ne offre uno) per le domande di orientamento o quando l'utente
  preferisce rispondere in fretta. Le opzioni sbagliate devono essere errori
  plausibili, non riempitivi.
- L'utente può rispondere "non lo so": è un'informazione utile, non un fallimento.

**Come valutare le risposte.**

- **Corretta:** confermalo in breve, aggiungendo al massimo un dettaglio che
  rafforza ("esatto — e nota che lo stesso controllo c'è anche in…").
- **Parziale:** riconosci la parte giusta, poi completa puntando al codice
  (`percorso:riga`).
- **Sbagliata o "non lo so":** non limitarti a dare la risposta. Rispiega quel pezzo
  da un'altra angolazione (un esempio concreto, il percorso di un dato reale lungo il
  flusso) e, dopo, fai una domanda diversa sullo stesso concetto per verificare che
  ora sia chiaro.

Se gli errori si concentrano su una parte, è la spiegazione di quella parte che non
ha funzionato: rifalla meglio, e se c'è una guida interattiva aggiornala (stessa
pagina, stesso indirizzo) e dillo.

**Chiusura.** Quando le domande sono finite, chiudi con un bilancio breve e onesto:

```
## Bilancio
- Chiaro: <concetti/parti su cui l'utente ha risposto bene>
- Da ripassare: <parti con errori o incertezze, con il file o il passo della guida
  da rileggere>
- Prossimo passo: <un suggerimento concreto in base allo scopo, es. "leggi i test
  in …", "prova a seguire una richiesta col debugger da …">
```

Se l'utente vuole saltare la verifica, rispetta la scelta: chiudi con il riepilogo
del flusso e ricordagli che puoi fargli le domande quando vuole.

## Domande dell'utente durante la spiegazione

L'utente può interromperti in qualsiasi momento con domande ("e questa funzione chi
la chiama?", "perché qui usano una coda?"). Rispondi leggendo il codice, con lo
stesso rigore tra fatto e inferenza. Se la domanda rivela che una parte della
spiegazione non è passata, tienine conto nelle domande di verifica.
