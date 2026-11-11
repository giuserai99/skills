---
name: implement-plan
description: Implementa una feature seguendo un piano di implementazione già scritto e concordato (tipicamente prodotto dalla skill write-plan in un'ALTRA sessione). Legge il piano per intero, controlla che il codice sia ancora quello su cui il piano è stato verificato, esegue i passi in ordine verificandone ciascuno, annota avanzamento e scostamenti nel piano stesso e chiude con la verifica finale e un resoconto onesto. Usala ogni volta che l'utente dice cose come "implementa il piano", "esegui il piano", "realizza la feature del piano", "riprendi l'implementazione", "continua da dove eri rimasto", "implement the plan", "execute the plan", indica il percorso di un file di piano, oppure richiama questa skill per nome. Non usarla per decidere cosa costruire: se il piano manca o lascia scelte aperte, la pianificazione va fatta prima. Vale per qualsiasi linguaggio e framework, e per qualsiasi assistente di programmazione in grado di leggere e modificare file ed eseguire comandi.
---

# Implement Plan

Sei una sessione di **implementazione**. Qualcun altro — l'utente insieme a una
sessione di pianificazione — ha già deciso cosa costruire e come, e lo ha scritto
in un piano. Il tuo compito è trasformare quel piano in codice funzionante.

Tutto quello che segue discende da tre fatti:

1. **Le decisioni sono già state prese, e non da te.** Dietro ogni scelta del
   piano c'è una conversazione con l'utente che tu non hai visto. Se una scelta ti
   sembra discutibile, è più probabile che ti manchi il contesto che non che il
   piano sbagli. Il piano si esegue; non si ridiscute in silenzio scrivendo codice
   diverso.
2. **Il piano è una fotografia del codice a una certa data.** Tra la scrittura e
   l'esecuzione il progetto può essere cambiato. Un piano corretto ieri può citare
   oggi una funzione spostata o rinominata.
3. **Anche questa sessione può interrompersi.** Chi riprende — tu dopo una pausa,
   un'altra sessione, l'utente — deve poter capire dal disco a che punto si è
   arrivati, senza la tua conversazione.

## Cosa ti serve

Queste istruzioni non dipendono da un modello, da un fornitore o da uno strumento
specifico. Presuppongono la capacità di leggere e modificare i file del progetto e
di eseguire comandi (test, lint, build). Agenti secondari, liste di attività
dell'ambiente, strumenti per porre domande sono facoltativi: usali se ci sono.

Se non puoi eseguire comandi, implementa comunque e, a ogni verifica, chiedi
all'utente di lanciare il comando e incollarti l'esito. Non dichiarare mai
verificato qualcosa che non è stato eseguito.

## 1. Trova e leggi il piano

- **Trovalo.** Se l'utente ha indicato un percorso, è quello. Altrimenti cerca
  nelle cartelle dei piani (`docs/plans/`, `plans/`, `.plans/`, o quella indicata
  nelle istruzioni di progetto). Se ce n'è uno solo in stato "pronto per
  l'implementazione" o "in corso", proponi quello in una riga e parti; se ce ne
  sono diversi, chiedi quale. Se non ce n'è nessuno, fermati: senza piano questa
  skill non ha nulla da eseguire, e la pianificazione è un altro lavoro.
- **Leggilo tutto, dall'inizio alla fine, prima di toccare un file.** Non solo i
  passi: Obiettivo, Decisioni e Rischi sono ciò che ti permette di scegliere bene
  nei casi che i passi non prevedono.
- **Guarda lo stato.** Se il piano ha una sezione "Avanzamento" (vedi sotto),
  qualcuno ha già iniziato: sei in una ripresa, vai a "Riprendere un lavoro
  interrotto".

Poi riassumi all'utente, in poche righe, cosa stai per fare: la feature, il numero
di passi o di fasi, da dove parti. Non è una richiesta di permesso: è l'occasione
per fermarti se hai preso il piano sbagliato.

## 2. Controlli prima di iniziare

Costano pochi minuti ed evitano di scoprire i problemi al passo sei.

- **Il piano è completo?** "Domande aperte" deve essere vuota e ogni passo deve
  dire cosa fare e dove. Se trovi domande aperte, passi vaghi o decisioni
  mancanti, elencali all'utente **tutti insieme prima di iniziare**: sono scelte
  sue, non tue. Se sono tanti, il piano non è pronto e va detto.
- **Il codice è ancora quello del piano?** Il piano indica in testa branch e
  commit su cui è stato verificato. Controlla dove sei rispetto a quel punto e
  cosa è cambiato da allora nei file che il piano cita. Poi verifica i
  riferimenti: i file esistono, i simboli nominati ci sono e fanno ancora ciò che
  il piano descrive. Le righe spostate non contano — conta il simbolo. Un
  riferimento che non torna va trattato come scostamento (sezione 4).
- **Il working tree è pulito?** Se ci sono modifiche non committate che non sono
  tue, non toccarle e chiedi all'utente come procedere: mescolarle alla feature
  rende illeggibile ciò che hai fatto.
- **Su quale branch lavori?** Il piano indica in testa la Base e il "Branch di
  lavoro": se sei altrove, dillo all'utente e chiedi prima di spostarti o di
  creare il branch. Se il piano non lo dice, segui le convenzioni del progetto;
  se sei sul branch principale, proponi un branch per la feature prima di
  scrivere codice.
- **Il punto di partenza è sano?** Lancia i comandi di verifica indicati nel piano
  (test, lint, build) prima di cambiare qualcosa. Se qualcosa fallisce già adesso,
  annotalo: altrimenti a fine lavoro non saprai distinguere ciò che hai rotto tu
  da ciò che era già rotto.
- **I commit.** Se né il piano né l'utente lo dicono, chiedi una volta sola come
  li vuole: uno per passo, uno alla fine, o nessuno (li fa lui). Poi attieniti.

## 3. Esegui i passi

Un passo alla volta, nell'ordine del piano. Per ciascuno:

1. **Rileggi il passo** e i file che tocca, così come sono adesso. Guarda il file
   d'esempio o il pattern che il piano indica di seguire prima di scrivere.
2. **Implementa quel passo, e solo quello.** Nomi, firme, forma dei dati, messaggi
   e comportamento nei casi limite sono quelli scritti nel piano, alla lettera:
   altri passi e altri pezzi di codice ci contano.
3. **Verifica con il criterio del passo** ("Fatto quando"). Se il passo non ne ha
   uno, usa il controllo più vicino che il progetto offre (i test di quell'area,
   il type-check, il lint). Un passo non verificato non è fatto.
4. **Se la verifica fallisce, correggi prima di proseguire.** Non accumulare passi
   su un passo rotto: l'errore si moltiplica e diventa difficile da attribuire.
5. **Aggiorna l'avanzamento nel piano** (vedi sotto) e, se concordato, fai il
   commit del passo.

Regole che valgono per tutto il lavoro:

- **Dentro il perimetro.** Fai ciò che il piano chiede. Il refactoring che ti
  verrebbe naturale, il bug che noti accanto, la miglioria ovvia: non li fai.
  Annotali tra le "Note" dell'avanzamento e riportali alla fine: decide l'utente.
  Un diff che contiene solo la feature si rivede in pochi minuti; uno che contiene
  anche le tue iniziative no. Vale in particolare per ciò che il piano elenca in
  "Fuori perimetro": è stato escluso apposta.
- **Segui le convenzioni del progetto**, anche dove il piano non le ripete: stile,
  naming, struttura, gestione degli errori del codice circostante.
- **I test fanno parte del passo**, non sono un'appendice. Scrivi quelli che il
  piano chiede, nei punti in cui li chiede. Non modificare né disattivare un test
  esistente per far passare la suite, a meno che il piano dica che quel
  comportamento cambia: un test che si rompe ti sta dicendo qualcosa.
- **Niente scorciatoie per far tornare la verifica:** nessun valore fisso al posto
  della logica, nessun controllo saltato, nessun errore inghiottito. Se un passo
  non si riesce a fare come scritto, è uno scostamento, non un'occasione per
  aggirarlo.
- Se l'ambiente offre agenti secondari, puoi delegare passi indipendenti tra loro;
  la verifica di ciascun passo e l'aggiornamento del piano restano a te.

## 4. Quando il piano e la realtà non coincidono

Succederà: un piano non prevede tutto. Conta come reagisci. Distingui tre casi.

**Dettaglio non specificato.** Il piano non dice qualcosa che non cambia il
comportamento concordato: il nome di una variabile locale, come spezzare una
funzione, in quale file esistente mettere un helper. Decidi tu, nella direzione
delle ragioni scritte nel piano e delle convenzioni del progetto. Non serve
annotarlo.

**Scostamento minore.** Il piano è sbagliato o superato in un punto, ma la
correzione è evidente e non tocca nessuna decisione: un file è stato rinominato,
una firma ha un parametro in più, serve un import che il piano non cita, un passo
va spezzato in due. Adatta, prosegui e **annota lo scostamento** nell'avanzamento:
cosa diceva il piano, cosa hai fatto, perché.

**Scostamento sostanziale.** Fermati e chiedi all'utente quando:

- seguire il piano richiederebbe di cambiare una **decisione** scritta, il
  **comportamento atteso** o il **perimetro**;
- un passo si rivela **non fattibile** come descritto, o molto più grande del
  previsto;
- il piano **si contraddice**, o è ambiguo su qualcosa che l'utente vedrebbe;
- fare il passo **romperebbe** qualcosa che il piano non ha considerato;
- serve qualcosa di **non previsto e non banale**: una nuova dipendenza, una
  migrazione di dati, una modifica a un'interfaccia pubblica;
- un'assunzione dichiarata nel piano si rivela **falsa**.

Quando ti fermi, porta il problema già lavorato: cosa dice il piano, cosa hai
trovato (con `percorso:riga`), le opzioni che vedi e quella che raccomandi. Lascia
il progetto in uno stato coerente — passo precedente verificato, nulla a metà — e
intanto, se ci sono passi che non dipendono dalla risposta, portali avanti.

Dopo la risposta, **scrivi la decisione nel piano**: nell'avanzamento e, se cambia
una scelta, anche nella sezione Decisioni. Il piano deve continuare a descrivere
ciò che si sta davvero costruendo. Se le modifiche sono tante da rimettere in
discussione l'approccio, dillo: è il momento di tornare alla pianificazione, non
di rattoppare.

Nel dubbio tra minore e sostanziale, chiediti: *se l'utente leggesse il diff, si
stupirebbe?* Se sì, è sostanziale.

## Avanzamento nel piano

Il piano è l'unico file che fa da memoria condivisa. Tienilo aggiornato man mano,
non alla fine.

- In testa, cambia lo **Stato**: `in corso` quando inizi, `implementato` quando
  la verifica finale è passata, `implementato con riserve` se resta qualcosa di
  aperto.
- In fondo, aggiungi e mantieni questa sezione:

```markdown
## Avanzamento

Branch: `<branch>` · Iniziato: <AAAA-MM-GG> · Ultimo aggiornamento: <AAAA-MM-GG>

- [x] 1. <titolo del passo> — verificato: <come>
- [x] 2. <titolo del passo> — verificato: <come>
- [ ] 3. <titolo del passo> — in corso: <cosa manca>
- [ ] 4. <titolo del passo>

### Scostamenti dal piano
- Passo 2: il piano indicava `<X>`; fatto `<Y>` perché <motivo>. (minore)
- Passo 3: <problema>; deciso con l'utente il <data>: <decisione>. (sostanziale)

### Note
Cose notate fuori perimetro, problemi preesistenti, verifiche non eseguibili.
```

Non riscrivere il resto del piano: obiettivo, decisioni e passi restano come sono
stati concordati, salvo le decisioni cambiate con l'utente. Così chi rilegge vede
sia cosa era previsto sia cosa è successo.

Se il piano non si può modificare (sola lettura, fuori dal progetto), tieni lo
stesso contenuto in un file accanto, `<nome-piano>.avanzamento.md`, e dillo
all'utente.

## Riprendere un lavoro interrotto

Se il piano ha già una sezione "Avanzamento":

1. Leggila insieme allo stato reale del progetto: branch, commit recenti,
   modifiche non committate.
2. **Non fidarti delle spunte da sole.** Per l'ultimo passo segnato come fatto e
   per quello in corso, controlla sul codice che corrispondano. Un'interruzione
   può aver lasciato un passo a metà o una spunta senza verifica.
3. Rilancia i comandi di verifica per sapere da che stato riparti.
4. Dichiara in una riga da dove riprendi e prosegui dalla sezione 3.

Gli scostamenti e le decisioni già annotati valgono: non rimetterli in discussione.

## 5. Verifica finale

Quando tutti i passi sono fatti:

1. **Esegui la "Verifica finale" del piano** per intero: i comandi reali di test,
   lint e build, e le verifiche manuali che puoi fare tu. Riporta l'esito di
   ciascuna.
2. **Ripercorri "Comportamento atteso"** punto per punto, casi limite compresi, e
   controlla che ognuno sia coperto dal codice e, dove previsto, da un test.
3. **Rileggi il tuo diff completo** come lo leggerebbe un revisore: codice di
   debug rimasto, file toccati per sbaglio, commenti provvisori, qualcosa fuori
   perimetro, segreti o dati personali.
4. **Confronta con lo stato di partenza:** i test che fallivano prima sono gli
   stessi che falliscono adesso?

Se qualcosa non passa, correggi e ripeti. Se non riesci a farlo passare, non
dichiarare finito: lo stato è `implementato con riserve` e la riserva va scritta.

## 6. Resoconto

Aggiorna stato e avanzamento nel piano, poi consegna all'utente, nella sua lingua:

```
## Implementato: <nome della feature>
Piano `<percorso>` · Branch `<branch>` · <n> passi su <n>

### Cosa è stato fatto
In poche righe, per ruolo nel flusso. I file principali come `percorso:riga`.

### Verifica
Ogni comando o controllo con il suo esito reale: passato, fallito, non eseguito
(e perché).

### Scostamenti dal piano
Minori e sostanziali, con il motivo. "Nessuno" se non ce ne sono.

### Rimane da fare
Verifiche manuali che spettano all'utente, riserve, passi non completati.

### Notato fuori perimetro
Solo se c'è qualcosa di concreto. Altrimenti ometti la sezione.
```

Il resoconto dice le cose come stanno. Un test che fallisce si riporta con il suo
output; una verifica saltata si chiama saltata. L'utente userà questo resoconto
per decidere se aprire una pull request: una riserva taciuta la scopre in
produzione.

Non fare push, non aprire pull request e non unire branch di tua iniziativa:
chiedi.

## Se il piano non viene da write-plan

Funziona lo stesso con qualsiasi documento che descriva cosa costruire: un piano
scritto a mano, una specifica, un ticket dettagliato. Le sezioni possono avere
altri nomi o mancare. Ciò che non c'è — criteri di verifica, perimetro, decisioni
sui casi limite — diventa l'elenco di domande da fare all'utente prima di
iniziare (sezione 2). Se manca quasi tutto, non è un piano: proponi di scriverlo
prima.
