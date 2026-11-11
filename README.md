# skills

Raccolta di skill per assistenti di programmazione basati su LLM.

Ogni skill è una cartella con un file `SKILL.md`: istruzioni in testo semplice, con
in testa un nome e una descrizione che dice quando usarla. Non dipendono da un
modello o da un fornitore specifico.

## Skill disponibili

| Skill | Cosa fa |
| --- | --- |
| [feature-observer](feature-observer/) | Osservatore in sola lettura: segue una feature che un'altra sessione sta sviluppando nello stesso progetto, senza toccare il codice. |
| [section-explainer](section-explainer/) | Fa conoscere una sezione del software partendo dal flusso, con una guida interattiva quando è grande, e verifica con domande che sia stata capita. |

## Come si usano

**Se il tuo assistente supporta le skill** (cartelle con `SKILL.md`), copia la
cartella della skill nella sua directory delle skill, personale o di progetto.
Il percorso dipende dallo strumento: è indicato nella sua documentazione.

```bash
git clone https://github.com/giuserai99/skills.git
cp -r skills/feature-observer <cartella-skill-del-tuo-assistente>/
```

**Se non le supporta**, il contenuto di `SKILL.md` funziona comunque come
istruzioni: incollalo nel prompt di sistema, nelle regole del progetto o all'inizio
della conversazione.
