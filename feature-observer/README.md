# feature-observer

Trasforma una sessione del tuo assistente di programmazione in un **osservatore in
sola lettura**: segue una feature che un'altra sessione sta sviluppando nello stesso
progetto locale, senza toccare il codice e senza appesantire il contesto della
sessione che lavora.

Non dipende dal linguaggio, dal framework, dal modello o dal fornitore: parte dal
diff e segue chi chiama chi. All'assistente servono solo due capacità, leggere i
file ed eseguire comandi di ispezione in una shell.

## A cosa serve

Mentre una sessione scrive codice, ogni domanda che le fai ("cosa hai cambiato?",
"perché quella validazione?") consuma il suo contesto. Con questa skill apri una
seconda sessione sullo stesso progetto e le domande le fai lì.

## Installazione

Vedi il [README della raccolta](../README.md#come-si-usano): copia questa cartella
tra le skill del tuo assistente, oppure usa il contenuto di `SKILL.md` come
istruzioni.

## Uso

1. Apri una seconda sessione sullo stesso progetto in cui un'altra sessione (o una
   persona) sta lavorando.
2. Richiama la skill per nome, oppure scrivi una frase come "segui la feature che
   sta facendo l'altra sessione".
3. Continua a parlare normalmente: a ogni tuo messaggio l'osservatore ricontrolla
   il progetto e ti dice cosa è cambiato.

## Cosa fa

- **Sola lettura.** Non crea né modifica file nel progetto e non lancia build, test
  o formatter. Di git usa solo comandi di ispezione con `--no-optional-locks`, per
  non bloccare un commit dell'altra sessione.
- **Primo aggancio.** Ricava dal disco branch, file toccati e intento, poi
  ricostruisce il flusso della feature dall'ingresso all'esito, segnando i passi
  presenti, in corso e mancanti.
- **Controllo a ogni messaggio.** Prima di rispondere verifica se qualcosa è
  cambiato. Se trova modifiche che non conosceva, corregge quanto aveva detto e
  ritara risposta e domande.
- **Checkpoint fuori dal progetto.** Salva uno snapshot a ogni controllo in una
  cartella temporanea e al successivo legge solo la differenza, commit compresi.
- **Lavoro a metà.** I file incompleti sono riportati come "in corso", non come
  errori, e i fatti osservati sono tenuti distinti dalle deduzioni.
- **Guida interattiva per le feature grosse.** Quando le modifiche sono tante,
  propone una pagina a passi con indice sempre presente: per ogni passo codice
  reale, consigli, tip e test da eseguire.

## Limiti

- Non esegue i test: li elenca per te o per la sessione che sviluppa.
- Senza git funziona con minore precisione, basandosi sulle date di modifica.
- Non scrive all'altra sessione, a meno che tu lo chieda.
- Agenti secondari, controlli a intervalli e pubblicazione della guida dipendono
  da ciò che offre il tuo assistente: dove mancano, l'osservatore fa lo stesso
  lavoro in modo più semplice.
