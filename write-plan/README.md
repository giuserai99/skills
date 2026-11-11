# write-plan

Trasforma una sessione del tuo assistente di programmazione in una **sessione di
pianificazione**: insieme a te arriva a un piano di implementazione completo di una
feature, verificato sulla codebase, che un'altra sessione potrà eseguire senza
dover chiedere nulla.

Non dipende dal linguaggio, dal framework, dal modello o dal fornitore.
All'assistente servono solo due capacità: leggere i file del progetto e scriverne
uno, il piano.

## A cosa serve

Quando si chiede a un assistente di implementare subito una feature, le domande
rimaste aperte le risolve lui, a modo suo, mentre scrive il codice. Con questa
skill le domande vengono fuori prima, si decidono insieme e finiscono scritte in un
documento. Chi implementa — un'altra sessione, con il contesto pulito — parte da
lì.

## Installazione

Vedi il [README della raccolta](../README.md#come-si-usano): copia questa cartella
tra le skill del tuo assistente, oppure usa il contenuto di `SKILL.md` come
istruzioni.

## Uso

1. Apri una sessione sul progetto e richiama la skill per nome, oppure scrivi una
   frase come "scriviamo il piano per questa feature".
2. Spiega cosa vuoi realizzare e rispondi alle domande: poche per volta, con
   opzioni e una raccomandazione.
3. Leggi la proposta con l'esito della verifica sul codice e conferma, o correggi.
4. Rileggi il piano salvato e passane il percorso alla sessione che implementa.

## Cosa fa

- **Parte dall'intento.** Ti chiede cosa vuoi realizzare e riformula ciò che ha
  capito prima di andare avanti.
- **Guarda il codice prima di chiedere.** Così le domande sono concrete e non
  riguardano ciò che il progetto dice già.
- **Domande fino a esaurirle.** Perimetro, comportamento, casi limite, dati,
  interfacce, vincoli, verifica: nulla resta da decidere a chi implementa.
- **Verifica di fattibilità.** Controlla punto per punto sul codice ciò che si è
  discusso e lo classifica: fattibile, fattibile ma diverso, non fattibile, non
  verificabile.
- **Proposta prima del piano.** Ti presenta approccio, problemi trovati e scelte
  ancora aperte; il piano si scrive solo dopo la tua conferma.
- **Piano autosufficiente.** Obiettivo, comportamento, decisioni con le loro
  ragioni, contesto nella codebase con `percorso:riga`, passi ordinati con il
  criterio di completamento, test e verifica finale.

## Limiti

- Non scrive codice: l'unico file che crea è il piano.
- Non avvia l'implementazione e non fa commit, a meno che tu lo chieda.
- Ciò che non si può verificare dal codice (sistemi esterni, dati di produzione)
  entra nel piano come assunzione dichiarata, non come certezza.
