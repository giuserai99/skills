# implement-plan

Trasforma una sessione del tuo assistente di programmazione in una **sessione di
implementazione**: prende un piano già scritto e concordato e lo realizza passo
per passo, verificando ogni passo e tenendo traccia nel piano stesso di cosa è
stato fatto e di cosa è andato diversamente.

È la seconda metà di [write-plan](../write-plan/): quella skill scrive il piano in
una sessione, questa lo esegue in un'altra. Non dipende dal linguaggio, dal
framework, dal modello o dal fornitore. All'assistente serve leggere e modificare
i file del progetto ed eseguire comandi.

## A cosa serve

Un piano concordato con cura vale poco se chi lo esegue lo reinterpreta: cambia
una scelta perché gli sembra migliore, aggiunge ciò che nessuno ha chiesto, salta
una verifica. Questa skill tiene l'implementazione aderente al piano, e quando il
piano e il codice non coincidono rende esplicito lo scostamento invece di
risolverlo in silenzio.

## Installazione

Vedi il [README della raccolta](../README.md#come-si-usano): copia questa cartella
tra le skill del tuo assistente, oppure usa il contenuto di `SKILL.md` come
istruzioni.

## Uso

1. Apri una nuova sessione sul progetto: non serve quella in cui hai scritto il
   piano.
2. Richiama la skill per nome indicando il piano, oppure scrivi una frase come
   "implementa il piano `docs/plans/2026-11-11-export-csv.md`".
3. Rispondi alle eventuali domande iniziali (branch, commit, punti poco chiari
   del piano) e lasciala lavorare. Ti interrompe solo se il piano non si può
   seguire com'è scritto.
4. Leggi il resoconto finale: cosa è stato fatto, esito delle verifiche,
   scostamenti, cosa resta a te.

Se la sessione si interrompe, aprine un'altra e scrivi "riprendi
l'implementazione del piano": riparte dall'avanzamento annotato nel file.

## Cosa fa

- **Legge tutto il piano prima di iniziare**, comprese decisioni e rischi.
- **Controlla il punto di partenza.** Piano completo, codice non cambiato rispetto
  al commit su cui il piano è stato verificato, working tree pulito, test già
  rossi annotati.
- **Un passo alla volta, ciascuno verificato** con il criterio scritto nel piano.
  Non prosegue su un passo rotto.
- **Resta nel perimetro.** Niente refactoring o migliorie non richieste: ciò che
  nota lo riporta alla fine.
- **Gestisce gli scostamenti per gravità.** I dettagli li decide, le correzioni
  evidenti le applica e le annota, per tutto ciò che tocca una decisione, il
  comportamento o il perimetro si ferma e chiede.
- **Avanzamento nel piano.** Stato, passi fatti, scostamenti e note sono scritti
  nel file: un'altra sessione può riprendere da lì.
- **Verifica finale e resoconto onesto.** Esegue i comandi reali del progetto e
  riporta gli esiti così come sono.

## Limiti

- Non decide cosa costruire: senza un piano, o con un piano pieno di punti aperti,
  rimanda alla pianificazione.
- Modifica il piano solo per stato, avanzamento e decisioni cambiate con te.
- Non fa push, non apre pull request e non unisce branch, a meno che tu lo chieda.
- Le verifiche manuali che richiedono te (un ambiente, un account, un occhio
  umano) le elenca, non le esegue.
