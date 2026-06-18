# SDS Beheer Systeem — Werkwijze

> Lees dit bestand in aan het begin van elke sessie waarin aan het SDS-systeem gewerkt wordt.
> Laatst bijgewerkt: 2026-06-15 (na v8p + datamerge).

---

## Locaties

| Wat | Pad |
|-----|-----|
| Repo root (index.html) | `~/Developer/sds_beheer/` |
| Versiebestanden | `~/Developer/sds_beheer/releases/` |
| Live URL | `https://robbezemer-training.github.io/sds-beheer` |
| GitHub repo | `robbezemer-training/sds-beheer` (publiek, Pages via main/root) |

**Naamgeving versiebestand:** `SDS_systeem_v[versie]_2026-[MM]-[DD].html`
**localStorage key:** `sds_v6` (gedeeld over alle versies)
**Google Apps Script sync-URL** is hardcoded in het systeem.

---

## Deploy naar GitHub Pages

> ⚠️ **Eerst data veiligstellen:** stuur altijd eerst de data vanuit de huidige versie naar Google Sheets vóór je een nieuwe versie deployt. Voorkomt dataverlies bij een localStorage-reset.

```bash
cd ~/Developer/sds_beheer && \
cp releases/SDS_systeem_v[versie]_2026-[MM]-[DD].html index.html && \
git add index.html && \
git commit -m "v[versie] — [korte omschrijving]" && \
git push
```

- GitHub Pages update: **±1–2 minuten** na push
- Cache omzeilen: **Cmd+Shift+R**
- Printen: **Chrome** → Cmd+P → **Marges: Geen** → Achtergrondafbeeldingen aan (niet Safari/WeasyPrint).

### Versie ophogen — LET OP
- Hoog **alleen** het UI-label op: de `<title>` (`Beheer v8x`) én de footer (`Beheer · v8x · Rotterdam 2026`).
- **NOOIT** een globale tekstvervanging (`replace("v8k","v8l")`) gebruiken om de versie te bumpen. De tekst `v8k` kan namelijk binnen de base64 van een logo/handtekening voorkomen → één letter verandert → "broken data stream" → corrupt beeld (cyaan glitch). Dit is in v8l misgegaan en in v8m hersteld.

---

## Rollen & codes

| Code | Rol | Rechten |
|------|-----|---------|
| *(geen)* | Geblokkeerd | — |
| `1234` | Vrijwilliger | Beperkt (Deelnemers/Vrijwilligers/Formulieren) |
| `2026` | Ingrid (projectleider) | Alles behalve Instellingen |
| `3162` | Rob (beheerder) | Alles |

---

## Datamodel (localStorage `db`)

```
db = { organisaties:[], workshops:[], deelnemers:[], vrijwilligers:[], evaluaties:[] }
```

- `ensureSchema()` zet `db.evaluaties = []` als die ontbreekt (migratie bij load + na elke Sheets-sync).
- **evaluaties** = platte lijst van lósse records:
  - deelnemer-formulier: `{id, wsId, type:'d', cijfer(1–10), geleerd, meer, onderwerp, aanbevelen, opmerkingen, ts}`
  - organisatie-evaluatie (1 per workshop): `{id, wsId, type:'o', contactpersoon, datum, samenwerking, aansluiting, opnieuw, beter, opmerkingen, ts}`
- Het systeem berekent zelf gemiddelden/percentages (`evalAggr(wsId)`).

---

## Evaluatie (sinds v8p)

- **Invoer per formulier** via 📝 Evaluatie op de workshopkaart: deelnemer-formulieren één voor één toevoegen (+ verwijderen), plus één organisatie-evaluatie per workshop.
- Ingevoerde formulieren staan als **nette bullet-lijst** op de kaart.
- **Printbare rapporten in huisstijl:**
  - 📊 Evaluatierapport per workshop (knop op kaart + Evaluatie-tab) → `printEvaluatieRapport(wsId)`
  - 🏛 Gemeente-evaluatie (Evaluatie-tab) → `printGemeenteEvaluatie()`
- Evaluatie-tab toont berekende cijfers (gem. cijfer, % aanbeveelt, % wil meer) totaal en per workshop.
- Vragen (vast): deelnemer V1 cijfer 1–10, V2 open, V3 Ja/Nee/Weet ik niet (+onderwerp), V4 Nee/Misschien/Ja/Zeker!, V5 open. Organisatie V1 samenwerking, V2 aansluiting, V3 opnieuw, V4 wat beter, V5 opmerkingen.

---

## Workshoprapport / formulieren — print

- Rapport portretpagina: **`min-height:auto`** (content-hoogte). NIET een vaste/min-hoogte forceren — dan wordt de voettekst over de A4-rand geduwd naar een lege pagina 2.
- Gekleurde lijnen altijd als **twee losse divs** (oranje/teal), geen CSS-gradient border-image (onbetrouwbaar bij print/PDF).
- Handtekeningen via **`mix-blend-mode: multiply`**.
- Certificaat = goedgekeurde `certificaat_v16f.html`; niet wijzigen zonder expliciete instructie.

---

## Data / Excel

- **De Excel `Registratie_deelnemers_en_vrijwilligers-*.xlsx` is planning-only** (tabs Aangeschreven, Bevestigd, en per-workshop bladen). De deelnemerbladen zijn lege templates — er staan **geen** deelnemers in.
- **Het systeem is de bron voor deelnemers.** Nooit de Excel kaal importeren: dat zou de deelnemers in het systeem wissen.
- Bij een Excel-update: alleen **aanvullen** via een **strikte superset** (huidige export + nieuwe records), nooit verwijderen. Werkwijze:
  1. In systeem: data uit Sheets ophalen (lokaal = Sheets).
  2. Exporteer huidige data (JSON).
  3. Merge tot superset (elk origineel record byte-exact behouden, alleen toevoegen).
  4. Importeren, controleren, naar Sheets pushen.

### Huidige datastand (backup 2026-06-14)
- 5→7 organisaties, 8 workshops, **21 deelnemers** (Charlois 10 + Pernis 11), 12→14 vrijwilligers, 0 evaluaties.
- Geleverde merge: `SDS_backup_samengevoegd_2026-06-14.json` (+2 vrijwilligers, +2 pipeline-orgs SOL Feijenoord/DOCK Hoogvliet als `aangeschreven`).

---

## Versiestand

- Laatst bevestigd gedeployed: **v8n** (commit 5623f56).
- **v8o** (rapport `min-height:auto`) en **v8p** (evaluatie-heropzet) gebouwd; deploy nog te bevestigen. **v8p vervangt v8o.**
- Releasebestand: `SDS_systeem_v8p_2026-06-13.html`.

---

## Open punten

1. **v8p deployen** + in Chrome controleren: per-formulier invoer, bullets, beide printbare evaluatierapporten, rapportprint (2 pagina's), deelnemer-eval op scherm.
2. **Samengevoegde JSON importeren** (eerst pull uit Sheets), controleren, pushen. Beslissen over de 2 pipeline-orgs (status `aangeschreven`; contactgegevens summier).
3. **CHANGELOG.md** bijwerken (geleverde versie met v8m–v8p in repo zetten).
4. **Sheets-sync voor evaluaties** — Apps Script moet het nieuwe `evaluaties`-veld aankunnen. Hangt samen met: alleen de beheerder kan naar Sheets pushen (Ingrid niet) — architectuur nog te bepalen.

---

## Multi-device
Rob werkt op iMac én laptop. Altijd **push naar Sheets vóór afsluiten** en **pull fresh data** bij opstarten op een ander apparaat.

---

## Huisstijl
- Kleuren: zwart/donker `#1C1E20`, oranje `#E8943A`, teal `#4DA99A`.
- Fonts: Bebas Neue / Poppins (titels), Source Sans 3 (body).
- Grotere fontgroottes voor leesbaarheid ouderen (min. 11–12pt), geen grijstinten in tekst.
