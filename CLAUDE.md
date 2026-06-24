# CLAUDE.md — Instruktioner för DINO ORCA-modellen

## Vad det här projektet är

Det här repot är en **domänmodell för systemet DINO**, modellerat med ORCA-metodik. ORCA är en strukturerad metod för att kartlägga domänobjekt med statusar, actions, roller och relationer — tänk en state machine per domänobjekt, dokumenterad som YAML.

Modellen används som gemensamt språk mellan verksamhet och teknik: vad ett uppdrag *är*, vilka *statusar* det kan ha, vilka *actions* som är möjliga i varje status, och vilka *roller* som får utföra dem.

---

## Tre grundregler — håll dessa alltid

1. **Git-repot är enda källan.** Aldrig Confluence, aldrig Miro. Versionshistorik och spårbarhet är inbyggt i Git.
2. **Du (Claude) är enda skrivaren.** Uppdateringar sker alltid via dig, aldrig manuellt i filerna. Ingen människa öppnar YAML-filerna direkt.
3. **Confluence är läsvy.** Den speglar innehållet men uppdateras aldrig direkt och är aldrig källan.

---

## Filstruktur

```
/
├── CLAUDE.md                          # Denna fil
├── README.md                          # Projektöversikt för människor
├── personas.yaml                      # Manuell fil, frikopplad från systemstrukturen
├── schema/
│   └── domain-object.schema.json      # JSON Schema — valideras vid varje push via GitHub Actions
├── domains/
│   └── uppdrag.yaml                   # Domänobjektfiler — en per objekt
├── generated/
│   └── roles.yaml                     # Genereras av dig från domains/*.yaml — ALDRIG manuellt redigerad
└── .github/
    └── workflows/
        └── validate.yml               # GitHub Action som validerar domains/*.yaml mot schemat
```

### Rollerna för varje fil

| Fil/katalog | Vem redigerar | Hur |
|---|---|---|
| `domains/*.yaml` | Enbart Claude | Via konversation — aldrig direkt |
| `generated/roles.yaml` | Enbart Claude | Regenereras automatiskt efter varje ändring i `domains/` |
| `schema/domain-object.schema.json` | Enbart Claude | Endast om schemat behöver utökas |
| `personas.yaml` | Teamet manuellt | Frikopplad fil, inte en del av state machine-modellen |
| `README.md` | Enbart Claude | Uppdateras när domänobjekttabellen ändras |

---

## Schemaprinciper

Dessa principer gäller för alla domänobjektfiler och måste följas vid varje uppdatering.

**Additivt schema — inga breaking changes.**
Sektionerna `attributes`, `actions`, `states` och `transitions` är listor man bara lägger till i — aldrig skriver om. Varje element har ett stabilt `id` (t.ex. `uppdrag.status.pausad`) som aldrig byter namn, även om `label` ändras. Konsumenter av modellen binder till `id`, inte till fritext.

**Versionshantering på objektnivå.**
Varje domänfil har ett `version`-fält (semantisk versioning) och en `changelog`-lista längst ner. Uppdatera dessa automatiskt vid varje förändring:
- **patch** (1.0.x) — ändrad beskrivning, nytt attribut, nytt villkor
- **minor** (1.x.0) — ny status eller ny transition
- **major** (x.0.0) — strukturell förändring som påverkar konsumenter

**`from` är alltid en lista.**
Även om en transition bara har ett möjligt från-tillstånd skrivs det alltid som lista: `from: [uppdrag.status.beställt]`.

**Villkor är alltid strukturerade.**
Villkor på transitions modelleras med `attribute`, `value`, `description` — aldrig som fritext.

**`to: ~` för actions utan statusändring.**
Transitions som inte ändrar status sätter `to: null` (YAML: `~`) och `status_change: false`.

---

## Tre ingångar för uppdateringar

### 1. Textbeskrivning
Användaren beskriver en förändring i text. Du tolkar och föreslår konkret YAML-diff för godkännande innan du skriver.

### 2. Nytt CSV/Miro-underlag
Användaren delar exportdata. Du jämför mot befintlig fil och lyfter fram avvikelser — vad som är nytt, vad som saknas, vad som skiljer sig.

### 3. Direkt i konversation
Användaren specificerar ändringen direkt. Du skriver YAML och checkar in.

**Flödet är alltid:** du föreslår → användaren godkänner → du checkar in. Committa aldrig utan godkännande.

---

## Hur du lägger till ett nytt domänobjekt

1. Läs `domains/uppdrag.yaml` som mall för korrekt struktur och namnkonvention.
2. Generera ny fil under `domains/<objektnamn>.yaml` med samma schema.
3. Visa filen för godkännande — committa inte direkt.
4. Efter godkännande: committa, regenerera `generated/roles.yaml`, uppdatera domänobjekttabellen i `README.md`.

---

## Hur du uppdaterar ett befintligt domänobjekt

1. Läs alltid den befintliga filen först.
2. Visa exakt vad som ändras (diff-format eller tydlig beskrivning av varje ändring).
3. Bumpa `version` och lägg till en rad i `changelog`.
4. Vänta på godkännande innan du skriver och committar.

---

## Vad du aldrig ska göra

- Ändra ett befintligt `id` — oavsett anledning.
- Ta bort ett befintligt element ur `states`, `transitions`, `attributes` eller `status_independent_actions`.
- Redigera `generated/roles.yaml` manuellt — den genereras alltid från `domains/`.
- Committa utan att användaren godkänt ändringen.
- Skriva fritext i fält som ska vara strukturerade (villkor, roller).
