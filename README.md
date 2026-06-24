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
│   └── roles.yaml                    # Genereras av Claude — redigeras ALDRIG manuellt
└── .github/
    └── workflows/
        └── validate.yml
```

---

## Domänobjekt

| Fil | Objekt | Version | Status |
|---|---|---|---|
| `domains/uppdrag.yaml` | Uppdrag | 1.0.0 | ✅ Aktiv |

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
