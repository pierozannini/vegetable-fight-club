## **Abstract**

*Vegetable Fight Club* è un micro-progetto “vibe coding” che genera ortaggi antropomorfizzati, assegna loro mosse di combattimento buffe e simula scontri casuali restituendo resoconti narrativi ed eventuali “card” collezionabili. Tutto avviene in un singolo pomeriggio, sfruttando un LLM per la parte creativa e poche decine di righe di Python (o JavaScript) per la logica e l’I/O.

---

## **Rationale**

1. **Creatività istantanea** – L’LLM sforna nomi, descrizioni e mosse memorabili (“Finocchio Katana-ista”, “Ravanello Acido”).
2. **Divertimento social** – Le card o i log di battaglia si possono postare su Slack/Discord.
3. **Scalabilità minima** – È un prototipo; niente database complessi né UI raffinata.
4. **Estendibilità** – In futuro puoi integrare immagini generate, leaderboard, modalità PVP.

---

## **Requisiti**

*Software*

* Python 3.11+
* Accesso a un’API LLM (Groq)
* Librerie: `requests`/`openai`, `jinja2` (templating), facoltativo `rich` per output colorato CLI

*Hardware*

* Qualsiasi laptop; RAM > 4 GB sufficiente.

*Tempo*

* \~3 h effettive di coding + 1 h di test/ritocchi.

---

## **Struttura del progetto**

```
vegetable-fight-club/
│
├── src/
│   ├── generator.py          # prompt & parsing output LLM
│   ├── simulator.py          # logica incontro e punteggi
│   ├── cli.py                # interfaccia a riga di comando
│   └── templates/
│       └── card.jinja        # HTML o Markdown per card
│
├── data/                     # salvataggi facoltativi
│   └── vegetables.json
│
├── README.md
└── requirements.txt
```

---

## **Descrizione componenti progetto**

| Componente               | Ruolo                                                                                                                                           | Note                                                        |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| **generator.py**         | Chiede all’LLM di creare n ortaggi (nome, specie, catchphrase, due mosse, valore “Power” 1-100) e salva JSON                                    | Prompt “system” + “user”; parsing semplice                  |
| **simulator.py**         | Seleziona due ortaggi random, calcola esito del combattimento in base al punteggio Power ± random δ e logga un racconto comico (LLM o template) | Algoritmo best-of-3 turni                                   |
| **cli.py**               | `python cli.py fight` o `python cli.py new 6`                                                                                                   | Usa `argparse` o `click`                                    |
| **templates/card.jinja** | Genera una card Markdown/HTML con emoji, colori e mini-lore                                                                                     | Può essere esportata in PNG via `wkhtmltoimage` se desideri |
| **data/**                | Cache locale per evitare rilanci d’API                                                                                                          | JSON leggibile                                              |

---

## **Roadmap di implementazione del codice**

| Fase                                                                                 | Durata stimata | Deliverable                              |
| ------------------------------------------------------------------------------------ | -------------- | ---------------------------------------- |
| **1. Setup progetto** – repo, venv, `requirements.txt`                               | 15 min         | Struttura directory pronta               |
| **2. Prompt & generation** – scrivi `generator.py`, genera 5 veggie per test         | 30 min         | `vegetables.json` con dati corretti      |
| **3. Simulatore rapido** – implementa funzione `fight(vegA, vegB)` in `simulator.py` | 40 min         | Log test su console                      |
| **4. CLI** – comandi `new`, `fight`, `show <id>`                                     | 30 min         | Interazione base funzionante             |
| **5. Card templating** – Jinja2 + Markdown/HTML export                               | 25 min         | File card.html/card.md per ogni ortaggio |
| **6. Polishing** – color output (`rich`), emoji, text wrap                           | 20 min         | UX simpatica                             |
| **7. Quick test session** – 10 match, catch edge-cases                               | 20 min         | Bug-fix                                  |
| **8. README & screenshot**                                                           | 20 min         | Repo presentabile                        |

*Totale: ≈ 3 ore 20 min.*

---

**Prossimi passi extra (facoltativi)**

* Integra Stable Diffusion / DALL-E per illustrare le card.
* Aggiungi leaderboard JSON e ranking Elo.
* Pubblica una mini-webapp su Replit/Streamlit per share pubblico.
