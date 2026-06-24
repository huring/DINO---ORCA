Säkerställ att repot är uppdaterat genom att köra `git pull` innan du läser några filer.

Läs sedan relevanta filer under `domains/`, `generated/` och `personas.yaml` och svara på frågan nedan om ORCA-modellen för DINO.

Fråga: $ARGUMENTS

---

Riktlinjer:

- Svara på svenska i ett mänskligt läsbart format — aldrig råa YAML-strukturer
- Läs bara de filer som är relevanta för frågan; läs inte hela repot i onödan
- Vid frågor om ett specifikt domänobjekt: läs dess fil under `domains/`
- Vid frågor om roller: läs `generated/roles.yaml` och `personas.yaml`
- Vid frågor om relationer: läs `generated/relations.yaml`
- Visa kardinalitet (1, 0..1, 0..*, 1..*) tydligt när du beskriver relationer
- Om en fråga rör en roll: inkludera vilka actions rollen får utföra och i vilka statusar
- Om något pekar mot ett objekt som ännu inte har en fil under `domains/` (forward reference): nämn det explicit
- Om frågan är oklar, be om förtydligande innan du läser filer
