# section-explainer

Fa conoscere una **sezione del software** — un modulo, una feature, un flusso, un
servizio — partendo dal suo flusso: da dove entra una richiesta, dove passa, dove
finisce. Quando la sezione è grande costruisce una **guida interattiva** a passi, e
alla fine **verifica con domande** che tu abbia capito davvero.

Non dipende dal linguaggio, dal framework, dal modello o dal fornitore: all'assistente
basta saper leggere i file del progetto.

## A cosa serve

Onboarding su una parte di codice che non conosci, preparazione a una modifica o a
un bugfix, revisione di un modulo scritto da altri. Invece di leggere file a caso,
segui il percorso reale dei dati, con il codice vero sotto gli occhi.

## Installazione

Vedi il [README della raccolta](../README.md#come-si-usano): copia questa cartella
tra le skill del tuo assistente, oppure usa il contenuto di `SKILL.md` come
istruzioni.

## Uso

1. Richiama la skill per nome, oppure scrivi una frase come "spiegami come funziona
   la fatturazione" o "fammi capire il flusso del login".
2. Se serve, l'assistente ti chiede in una riga perimetro e scopo (modificarla,
   correggere un bug, onboarding).
3. Leggi la spiegazione in chat o la guida interattiva, fai domande quando vuoi.
4. Rispondi alle domande di verifica: alla fine ricevi un bilancio di cosa è chiaro
   e cosa ripassare.

## Cosa fa

- **Sola lettura.** Non modifica il progetto; eventuali file di appoggio stanno in
  una cartella temporanea.
- **Parte dal flusso.** Trova gli ingressi, segue chi chiama chi fino all'esito,
  segna dati, bivi ed errori, e distingue i fatti osservati dalle deduzioni.
- **Due formati.** Sezione piccola: spiegazione in chat con diagramma e
  riferimenti `file:riga`. Sezione grande: guida a passi con mappa del flusso
  cliccabile, codice reale, bivi esplorabili e glossario.
- **Verifica della comprensione.** 4–6 domande ancorate al codice, a difficoltà
  crescente (orientamento, flusso, bivi, applicazione), una o due alla volta. Le
  risposte sbagliate portano a una nuova spiegazione, non solo alla soluzione.

## Limiti

- Spiega il codice com'è: per cambiarlo serve un altro compito.
- Pubblicazione della guida, agenti secondari e domande a scelta multipla dipendono
  da ciò che offre il tuo assistente: dove mancano, la skill genera un file HTML
  locale e fa le domande in chat.
