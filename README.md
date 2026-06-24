# DINO — ORCA-modell

Domänmodell för DINO, modellerad med ORCA-metodik. Innehållet är strukturerat som YAML-filer per domänobjekt.

---

## Principer

| Lager | Roll |
|---|---|
| **Git-repo (denna fil)** | Enda källan till sanning. Versionshistorik och spårbarhet är inbyggt. |
| **Claude** | Enda skrivaren. Uppdateringar sker via Claude, aldrig manuellt i filerna. |
| **Confluence** | Läsvy. Pekar på eller speglar innehållet, men är aldrig källan. |

**Ingen människa redigerar YAML-filerna direkt.** Alla ändringar initieras som en konversation med Claude, som föreslår en konkret ändring — du godkänner — Claude checkar in.

---

## Filstruktur

```
orca-model/
├── README.md
├── personas.yaml                     # Manuell fil, frikopplad från systemstrukturen
├── schema/
│   └── domain-object.schema.json     # JSON Schema — valideras vid varje push
├── domains/
│   ├── uppdrag.yaml
│   └── ...                           # Framtida domänobjekt
├── generated/
│   ├── roles.yaml                    # Genereras av Claude — redigeras ALDRIG manuellt
│   └── relations.yaml                # Genereras av Claude — redigeras ALDRIG manuellt
├── skills/
│   ├── orca-fraga.skill              # /orca-fråga — fråga om modellen (Claude Co-work)
│   └── orca-andra.skill              # /orca-ändra — uppdatera modellen (Claude Co-work)
├── .claude/
│   └── commands/
│       ├── orca-fråga.md             # /orca-fråga — Claude Desktop / Claude Code
│       └── orca-ändra.md             # /orca-ändra — Claude Desktop / Claude Code
└── .github/
    └── workflows/
        └── validate.yml
```

---

## Domänobjekt

| Fil | Objekt | Version | Status |
|---|---|---|---|
| `domains/uppdrag.yaml` | Uppdrag | 1.0.2 | ✅ Aktiv |
| `domains/delomrade.yaml` | Delområde | 1.0.2 | ✅ Aktiv |

---

## Kommandon

Repot innehåller två kommandon som installeras automatiskt när du öppnar mappen i Claude.

| Kommando | Syfte | Tillgänglig i |
|---|---|---|
| `/orca-fråga` | Ställ frågor om modellen | Claude Desktop, Claude Code, Claude Co-work |
| `/orca-ändra` | Uppdatera modellen (föreslår → godkänner → committar) | Claude Desktop, Claude Code, Claude Co-work |

**Exempel:**
```
/orca-fråga vilka actions kan en Fördelare utföra?
/orca-ändra lägg till rollen Handläggare på Delområde
```

Fullständig dokumentation: [Kom igång med DINO ORCA-modellen](https://metria-nv.atlassian.net/wiki/x/CID8NQ)

---

## Lägga till ett nytt domänobjekt

1. Samla underlag (Miro-export, CSV eller textbeskrivning)
2. Beskriv domänobjektet för Claude
3. Claude genererar ny fil under `domains/` enligt schemat
4. Granska och godkänn
5. Claude checkar in och regenererar `generated/roles.yaml`

## Uppdatera ett befintligt domänobjekt

Tre ingångar — alla hanteras av Claude:

- **Textbeskrivning** — beskriv förändringen, Claude föreslår YAML-diff
- **Nytt CSV/Miro-underlag** — Claude jämför mot befintlig fil och lyfter fram avvikelser
- **Direkt i konversation** — specificera ändringen, Claude skriver och checkar in

Flödet är alltid: Claude föreslår → du godkänner → Claude checkar in.
