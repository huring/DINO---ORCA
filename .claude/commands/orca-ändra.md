Du är enda skrivaren för DINO ORCA-modellen. Följ alltid nedanstående flöde exakt — avvik aldrig från det.

Instruktion: $ARGUMENTS

---

## Flöde — följ alltid dessa steg i ordning

**1. Läs reglerna**
Läs `CLAUDE.md` innan du gör något annat.

**2. Läs berörda filer**
Identifiera vilka filer under `domains/` som berörs av instruktionen och läs dem.

**3. Föreslå ändringen**
Presentera exakt vad som ska ändras — som en tydlig beskrivning eller YAML-diff. Inkludera:
- Vilka fält som läggs till, ändras eller (aldrig) tas bort
- Ny version och changelog-rad
- Eventuella följdändringar i `generated/`-filer eller `personas.yaml`

**4. Vänta på godkännande**
Skriv ingenting till disk och committa ingenting förrän användaren explicit godkänt förslaget.
Godkännande = ett tydligt "ja", "godkänt", "gör det" eller motsvarande.
Tveksamhet eller tystnad = inte godkänt.

**5. Genomför och validera**
- Skriv ändringarna till rätt filer
- Kör validering: `python -m jsonschema` eller motsvarande mot `schema/domain-object.schema.json`
- Åtgärda eventuella valideringsfel innan du går vidare

**6. Committa**
Committa med ett beskrivande meddelande. Pusha aldrig utan att användaren explicit ber om det.

---

## Regler som alltid gäller

- Ändra aldrig ett befintligt `id` — oavsett anledning
- Ta aldrig bort ett befintligt element ur `states`, `transitions`, `attributes`, `status_independent_actions` eller `relations`
- Bumpa alltid `version` (patch/minor/major) och lägg till en rad i `changelog`
- Regenerera `generated/roles.yaml` och `generated/relations.yaml` om roller eller relationer påverkas
- Synka `personas.yaml` om nya roller tillkommer
- Validera alltid mot `schema/domain-object.schema.json` innan commit
