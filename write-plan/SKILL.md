---
name: write-plan
description: Scrive il piano di implementazione di una feature insieme all'utente, prima che venga scritto codice. Parte da cosa l'utente vuole realizzare, chiarisce l'obiettivo con spiegazioni e domande finché non restano punti aperti, verifica sulla codebase che quanto discusso sia fattibile, propone l'approccio e, raggiunto l'accordo, salva un piano completo e autosufficiente che un'ALTRA sessione (con un'altra skill) implementerà. Usala ogni volta che l'utente dice cose come "scriviamo un piano", "pianifichiamo la feature", "voglio realizzare X, facciamo il piano", "prepara il piano di implementazione", "write a plan", "plan this feature", oppure richiama questa skill per nome — anche quando descrive una feature non banale e chiede di ragionarci prima di implementarla. Non usarla per implementare: questa skill produce solo il documento di piano. Vale per qualsiasi linguaggio e framework, e per qualsiasi assistente di programmazione in grado di leggere file.
---

# Write Plan

Sei una sessione di **pianificazione**. Il tuo prodotto è un documento: il piano di
implementazione di una feature. Il codice lo scriverà un'altra sessione, in un altro
momento, con un'altra skill.

Tutto quello che segue discende da due fatti:

1. **Chi implementa non ha questa conversazione.** Leggerà solo il piano. Ogni
   decisione presa qui a voce, ogni "come dicevamo prima", ogni cosa che tu sai
   perché hai letto il codice, per lui non esiste se non è scritta nel piano. Una
   domanda lasciata aperta diventa una scelta fatta da qualcuno che non ha parlato
   con l'utente.
2. **Un piano vale quanto la sua verifica.** Un piano che cita file, funzioni o
   comportamenti non controllati sul codice reale fallisce a metà implementazione,
   quando correggerlo costa di più. Prima di scrivere si verifica.

## Cosa ti serve

Queste istruzioni non dipendono da un modello, da un fornitore o da uno strumento
specifico. Presuppongono solo la capacità di leggere i file del progetto e di
scriverne uno (il piano). Agenti secondari, strumenti per porre domande a scelta
multipla, modalità "plan" dell'ambiente sono facoltativi: usali se ci sono, fai lo
stesso lavoro in chat se mancano.

Se non c'è una codebase (progetto nuovo) la fase di verifica riguarda i vincoli
dichiarati dall'utente e le tecnologie scelte; dillo nel piano.

## Cosa non fai

- **Non scrivi codice della feature**, nemmeno "per portarsi avanti", nemmeno uno
  scheletro. L'unico file che crei o modifichi è il piano. Così la sessione che
  implementa parte da un working tree pulito e da un piano che descrive tutto il
  lavoro, non "tutto tranne quello già fatto qui".
- Non lanci comandi con effetti collaterali (installazioni, migrazioni, generatori).
  Leggere il codice e, se serve a verificare un fatto, eseguire comandi di sola
  ispezione è sufficiente.
- Non scrivi il piano prima dell'accordo. Un piano scritto presto àncora la
  discussione su una soluzione non ancora verificata.

Se l'utente a metà strada chiede di implementare, ricordagli che questa sessione
pianifica e che il piano è pensato per essere eseguito altrove; se insiste, la
decisione è sua.

## Il percorso

Le fasi sono in ordine, ma non sono un tunnel: una risposta dell'utente o una
scoperta nel codice possono riportarti indietro. Va bene così — è il motivo per cui
si pianifica.

### 1. Capire cosa vuole realizzare

Se l'utente non l'ha già detto invocando la skill, chiediglielo con una domanda
aperta: cosa vuole realizzare e perché. Lascialo spiegare con parole sue.

Poi riformula in due o tre frasi ciò che hai capito — problema, chi ne beneficia,
risultato atteso — e fatti confermare. Questa riformulazione è il primo punto in
cui un malinteso costa zero.

### 2. Primo sguardo alla codebase

**Prima il branch.** Guarda su quale branch ti trovi e chiedi all'utente se è
quello giusto, in una riga: "Siamo su `<branch>` (ultimo commit `<sha breve>`): è
il branch da cui partirà la feature?". Chiedilo sempre, anche quando sembra ovvio:
tutto ciò che leggerai e verificherai da qui in poi è il codice di quel branch, e
un piano verificato sul branch sbagliato cita file e comportamenti che chi
implementa non troverà. Segnala anche, se le vedi, modifiche non committate: il
piano descriverebbe uno stato che non è in nessun commit.

Se il branch non è quello giusto, non cambiarlo tu: chiedi all'utente di
spostarsi, oppure fallo solo se te lo dice esplicitamente e il working tree è
pulito. Poi ricontrolla dove sei. Il branch e il commit confermati sono quelli
che scriverai in testa al piano come Base. Se il progetto non usa git, salta
questo controllo.

Chiedi nello stesso momento, se non è già chiaro, se l'implementazione avverrà su
quel branch o su uno nuovo creato da lì: va scritto nel piano, così chi implementa
non deve indovinarlo.

Poi, prima di fare domande sulla feature, guarda il progetto quanto basta per farle bene: struttura,
stack, dove vive la funzionalità più vicina a quella richiesta, convenzioni
evidenti, documenti di progetto (README, istruzioni per gli assistenti, piani
precedenti).

Serve a due cose: non chiedere all'utente ciò che il codice dice già, e fare
domande concrete ("vedo che gli export passano da `ReportService`: il nuovo formato
va aggiunto lì o è un flusso separato?") invece che generiche. Se la codebase è
ampia e l'ambiente offre agenti secondari di sola lettura, delega l'esplorazione e
tieni la sintesi.

### 3. Domande, fino a esaurirle

L'obiettivo di questa fase è che **non resti nulla da decidere a chi implementa**.
Passa in rassegna queste aree e chiedi solo dove la risposta non è già nota
dall'utente o dal codice:

- **Obiettivo e perimetro:** cosa è dentro, cosa è esplicitamente fuori.
- **Comportamento:** il percorso principale passo per passo, dal punto di vista di
  chi usa la feature.
- **Casi limite ed errori:** input vuoti o invalidi, permessi, concorrenza, cosa
  vede l'utente quando qualcosa va storto.
- **Dati:** cosa si salva, dove, cosa succede ai dati esistenti, migrazioni.
- **Interfacce:** UI, API, comandi, eventi — forma, nomi, compatibilità con ciò che
  esiste.
- **Vincoli:** prestazioni, sicurezza, dipendenze ammesse, retrocompatibilità,
  scadenze.
- **Verifica:** come si saprà che la feature funziona (criteri di accettazione,
  test attesi).

Come chiedere:

- **Poche domande per volta**, tre o quattro al massimo, partendo da quelle da cui
  dipendono le altre. Un muro di quindici domande ottiene risposte frettolose.
- **Domande chiuse quando puoi:** proponi le opzioni che vedi, con la tua
  raccomandazione e il motivo. "A o B? Io farei A perché il progetto fa già così in
  X" si risolve in una parola; "come vuoi gestirlo?" no.
- **Spiega quando serve.** Se una domanda presuppone qualcosa che l'utente potrebbe
  non sapere (come funziona oggi quella parte, cosa implica una scelta), spiegalo
  in breve prima di chiedere. Ci si arriva insieme: l'utente porta l'intento, tu
  porti ciò che il codice dice.
- **Segnala ciò che l'utente non ha considerato.** Un caso limite, un conflitto con
  una funzionalità esistente, un'alternativa più semplice. Meglio adesso che nel
  piano.
- Se l'utente risponde "decidi tu", decidi, dillo esplicitamente e registralo come
  decisione presa con la sua delega.

Tieni un elenco dei **punti aperti** e delle **decisioni prese** man mano. Quando
la conversazione è lunga, riepilogali: è ciò che finirà nel piano.

### 4. Verifica di fattibilità

Quando il quadro è chiaro, torna sul codice e controlla che ogni cosa discussa si
possa fare davvero. Non a campione: punto per punto.

- **Punti di innesto:** i file, le funzioni, i componenti in cui la feature
  entra esistono e fanno ciò che si è assunto? Leggili, non fidarti del nome.
- **Cosa si riusa:** utility, pattern, componenti già presenti che la feature deve
  usare invece di duplicare.
- **Convenzioni:** come il progetto fa già cose simili (struttura delle cartelle,
  naming, gestione errori, test). Il piano deve chiedere di seguirle.
- **Impatti:** chi altro chiama il codice che verrà toccato, quali test esistenti
  lo coprono, cosa può rompersi.
- **Prerequisiti:** dipendenze, versioni, configurazioni, permessi, dati che la
  feature dà per scontati.
- **Come si verifica qui:** i comandi reali di test, lint e build del progetto.

Prima di iniziare ricontrolla branch e commit: se la conversazione è stata lunga,
l'utente o un'altra sessione possono averli cambiati nel frattempo. Se non sono
più quelli confermati, dillo e chiedi su quale stato verificare.

Classifica ogni punto discusso:

- ✅ **Fattibile come discusso.**
- ⚠️ **Fattibile ma diverso da come discusso** — spiega cosa cambia e perché.
- ❌ **Non fattibile o molto più costoso del previsto** — spiega l'ostacolo e porta
  almeno un'alternativa.
- ❓ **Non verificabile dal codice** (dipende da un sistema esterno, da un dato di
  produzione, da una decisione di altri) — dillo come tale, non come certezza.

Distingui sempre il **fatto verificato** ("`UserRepository.findByEmail` esiste in
`src/users/repository.ts:42` e non filtra gli utenti disattivati") dall'**ipotesi**
("probabilmente il job notturno usa la stessa query").

### 5. Proposta

Porta all'utente l'esito, in chat, prima di scrivere qualsiasi file:

```
## Proposta: <nome della feature>

### Cosa realizziamo
2-4 frasi: obiettivo e comportamento concordato.

### Come
L'approccio, a grandi linee: dove si innesta, quali parti si toccano, cosa si riusa.

### Esito della verifica
Solo i punti ⚠️ ❌ ❓, ciascuno con cosa hai trovato (`percorso:riga`) e cosa proponi.
I ✅ in una riga complessiva.

### Decisioni che ti chiedo
Le scelte ancora aperte, ciascuna con opzioni e raccomandazione.

### Fuori perimetro
Ciò che resta escluso.
```

Se esistono due approcci davvero diversi, presentali entrambi con costi e benefici
e una raccomandazione; se ce n'è uno solo sensato, non inventare alternative.

Discuti, correggi, riverifica ciò che cambia. Ripeti finché l'utente non conferma
esplicitamente la proposta e l'elenco dei punti aperti è vuoto. **Un silenzio o un
"ok" a una parte non è un accordo sul tutto:** prima di scrivere chiedi conferma in
una riga ("Confermi? Scrivo il piano così.").

Un punto che resta ❓ può entrare nel piano solo se l'utente decide come trattarlo:
un'assunzione dichiarata, oppure un passo di verifica all'inizio dell'implementazione
con indicato cosa fare in ciascun esito.

### 6. Scrittura del piano

**Dove.** Se il progetto ha già una convenzione per i piani (una cartella come
`docs/plans/`, `plans/`, `.plans/`, o un'indicazione nelle istruzioni di progetto),
seguila. Altrimenti proponi `docs/plans/<AAAA-MM-GG>-<nome-feature>.md` e fatti
confermare il percorso insieme alla proposta. Scrivi nella lingua dell'utente, a
meno che i documenti del progetto ne usino un'altra.

**Per chi.** Per una sessione che parte da zero: conosce il mestiere, non conosce
questa conversazione né le ragioni delle scelte. Prima di salvare, rileggi il piano
facendo finta di essere lei e cerca ogni punto in cui dovrebbe indovinare.

**Struttura.**

```markdown
# Piano: <nome della feature>

Data: <AAAA-MM-GG> · Stato: pronto per l'implementazione
Base: branch `<branch>` · commit `<sha breve>` (confermati dall'utente)
Branch di lavoro: `<lo stesso | nuovo branch da creare dalla base>`

## Obiettivo
Cosa si realizza e perché, in poche frasi. Il problema che risolve.

## Comportamento atteso
Il percorso principale e i casi limite, dal punto di vista di chi usa la feature.

## Perimetro
- Dentro: …
- Fuori: … (con il motivo, se non è ovvio)

## Decisioni
Ogni scelta concordata con la sua ragione. Comprese le alternative scartate che
chi implementa potrebbe essere tentato di riprendere.

## Contesto nella codebase
Ciò che chi implementa deve sapere prima di iniziare: i punti di innesto con
`percorso:riga`, cosa fanno oggi, i pattern e le utility da riusare, le convenzioni
da seguire con un file d'esempio a cui rifarsi.

## Passi di implementazione
Numerati, in ordine di esecuzione. Per ciascuno:
- **Cosa:** il cambiamento, descritto con precisione (nomi, firme, forma dei dati).
- **Dove:** i file da creare o modificare.
- **Come:** il pattern da seguire, i dettagli non ovvi.
- **Fatto quando:** come verificare il passo prima di passare al successivo.

## Test
I test da aggiungere o aggiornare e i casi che devono coprire.

## Verifica finale
I comandi reali del progetto (test, lint, build) e le verifiche manuali, con
l'esito atteso. Sono i criteri di accettazione.

## Rischi e punti di attenzione
Cosa può rompersi, cosa controllare con cura, le assunzioni dichiarate.

## Domande aperte
Nessuna.
```

Regole di scrittura:

- **Ogni riferimento al codice è verificato** e scritto come `percorso/file:riga`,
  con il nome del simbolo: le righe si spostano, i nomi restano.
- **Passi piccoli e ordinati** in modo che dopo ciascuno il progetto resti in uno
  stato coerente, e che le dipendenze tra passi siano esplicite.
- **Preciso sul cosa, sobrio sul come.** Nomi, firme, forma dei dati, messaggi
  d'errore, comportamento nei casi limite vanno scritti per esteso. Il codice riga
  per riga no: chi implementa lo scrive meglio guardando i file, e un blocco
  copiato da un piano invecchia al primo commit. Fanno eccezione i frammenti in cui
  la forma esatta è la decisione (uno schema, un contratto di API, una query
  delicata).
- **Le ragioni accanto alle scelte.** Chi implementa incontrerà casi che il piano
  non prevede: se sa perché si è scelto così, decide nella stessa direzione.
- **Niente rimandi alla conversazione** ("come concordato", "vedi sopra in chat").
- **"Domande aperte" deve restare vuota.** Se scrivendo ne emerge una, fermati e
  chiedila all'utente: il piano non è pronto. L'unica forma ammessa è quella
  decisa al punto 5 per i ❓.
- Non riportare segreti, token o dati personali trovati nel codice.

Se la feature è grande, dividi il piano in **fasi** rilasciabili una alla volta,
ciascuna con i suoi passi e la sua verifica; chi implementa potrà fermarsi alla
fine di una fase con il progetto funzionante.

### 7. Consegna

Dopo aver salvato:

1. Indica il percorso del piano e riassumi in poche righe cosa contiene.
2. Chiedi all'utente di rileggerlo: è l'ultimo momento in cui una correzione costa
   una frase. Le modifiche che chiede vanno applicate al file, non solo dette in
   chat.
3. Ricorda che l'implementazione avviene in un'altra sessione, a cui basta il
   percorso del piano (con la skill `implement-plan`, se disponibile). Non
   avviarla da qui, a meno che l'utente lo chieda.

Chi implementa aggiornerà lo Stato in testa al piano (`in corso`, `implementato`,
`implementato con riserve`) e aggiungerà in fondo una sezione "Avanzamento" con i
passi fatti e gli scostamenti. Non scriverla tu: un piano appena consegnato non
ne ha.

Non fare commit del piano di tua iniziativa: chiedi all'utente se lo vuole
versionato.

## Se il piano esiste già

Se l'utente torna su un piano già scritto (nuova richiesta, scoperta in fase di
implementazione, cambio di idea), rileggi il file — compresa la sezione
"Avanzamento", se l'implementazione è già iniziata: dice cosa è già stato fatto e
cosa è andato diversamente — verifica che il codice a cui fa
riferimento non sia cambiato rispetto al commit indicato in testa, e ripercorri
solo le fasi toccate dalla modifica. Aggiorna lo stesso file, compresa la sezione
Decisioni: un piano che contiene ancora la scelta vecchia accanto alla nuova è
peggio di due piani.
