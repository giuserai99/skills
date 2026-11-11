# ticket-resolver

Dato un problema (un ticket, una segnalazione di bug, un messaggio d'errore), lo
riconduce al codice: trova la sezione coinvolta, ne cerca la causa e propone le
soluzioni. Prima di diagnosticare **chiede come procedere**: con una spiegazione
di come funziona quella sezione, oppure andando dritto al problema.

Non dipende dal linguaggio, dal framework, dal modello o dal fornitore.
All'assistente serve solo poter leggere e cercare nei file; eseguire comandi
(test, git) migliora la diagnosi ma è facoltativo.

## A cosa serve

Chi prende in carico un ticket non sempre conosce il codice che lo riguarda. Con
il percorso guidato ottieni prima la mappa di quella sezione e poi la diagnosi,
così puoi valutarla e spiegarla a tua volta. Se il codice lo conosci già, il
percorso diretto ti porta subito causa e soluzioni.

## Installazione

Vedi il [README della raccolta](../README.md#come-si-usano): copia questa cartella
tra le skill del tuo assistente, oppure usa il contenuto di `SKILL.md` come
istruzioni.

## Uso

1. Richiama la skill per nome, oppure incolla il ticket con una frase come
   "risolvi questo ticket" o "perché succede questo?".
2. L'assistente individua la sezione di codice coinvolta e ti chiede:
   **guidato** o **diretto**.
3. Ricevi causa e soluzioni; se vuoi, gli chiedi di applicare quella consigliata.

Per saltare la domanda dillo subito: "spiegami prima come funziona" oppure "vai
dritto al problema".

## Cosa fa

- **Localizza.** Parte dalle stringhe esatte del ticket (errore, etichette, URL,
  stack trace) e segue chi chiama chi fino a delimitare la sezione.
- **Chiede la strada.** Una sola domanda, dopo aver trovato la sezione e prima di
  diagnosticare.
- **Guidato.** Spiega a cosa serve la sezione, il flusso passo per passo con
  `file:riga` e i punti delicati; poi si ferma, così puoi indicare tu dove
  sospetti l'anomalia.
- **Diagnosi.** Formula ipotesi, le verifica sul codice e sulla storia git, le
  scarta esplicitamente, e controlla che la causa spieghi tutto il sintomo.
  Distingue i fatti osservati dalle deduzioni e dichiara il grado di certezza.
- **Soluzioni.** Per ogni causa: cosa cambiare, impatto sugli altri chiamanti,
  rischi, come verificare. Ne consiglia una.
- **Applica solo su richiesta.** Non modifica il codice finché non glielo chiedi.

## Limiti

- Con un ticket vago e senza appigli nel codice chiede un dettaglio in più
  invece di tirare a indovinare.
- Senza la possibilità di eseguire comandi non riproduce il problema: la causa
  resta dimostrata dalla sola lettura del codice, e lo dichiara.
- Se la causa è nei dati, nella configurazione o in un servizio esterno, indica
  dove guardare ma non può verificarlo da sé.
- Non fa commit né push se non richiesto.
