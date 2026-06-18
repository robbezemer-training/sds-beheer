# CHANGELOG — SDS Beheer Systeem

| Versie | Datum | Data | Wijzigingen |
|--------|-------|------|-------------|
| v6 | 2026-06-08 | data v1 | Eerste werkende versie, basisstructuur |
| v7 | 2026-06-08 | data v1 | Organisaties als centraal object, kaartenbak, flow indicator |
| v8 | 2026-06-08 | data v1 | Vier-rollen login, presentielijst afdrukken, collapsible deelnemers |
| v8a | 2026-06-09 | data v1 | Printknop op workshopkaart, kolomlijnen + vinkvakjes in presentielijst |
| v8b | 2026-06-10 | data v2 | Edit knoppen op alle kaarten, uitklap deelnemers/vrijwilligers op workshopkaart, deelnemer + vrijwilliger toevoegen vanuit workshop |
| v8e | 2026-06-10 | data v2 | Organisaties kaartenbak + presentielijst + vier rollen (verdere iteratie) |
| v8f | 2026-06-10 | data v2 | Organisaties kaartenbak + presentielijst + vier rollen (vervolg) |
| v8i | 2026-06-10 | data v2 | D2-aanwezigheidslijst in formulierstijl, aanwezigheid met datums |
| v8j | 2026-06-11 | data v2 | Aanwezigheid als tekst i.p.v. checkboxen |
| v8k | 2026-06-12 | data v2 | Certificaat hersteld (= goedgekeurde v16f: logo's + handtekeningen als afbeeldingen, gekleurde balken/lijnen als losse divs, vier lijntjes uitgelijnd, datumbug gefixt). Knop 🖨 Workshoprapport bij Formulieren (2 pagina's: overzicht + deelnemerstabel). Leeftijd in uitgeklapte deelnemerslijst. |
| v8l | 2026-06-12 | data v2 | Bugfix: afgekapt RB-logo in de 3 formulieren vervangen. Bugfix: Workshoprapport-knop werkte niet (ontbrekende CERT_LOGO-consts toegevoegd). |
| v8m | 2026-06-13 | data v2 | Cyaan glitch Mens Centraal-logo hersteld op alle formulieren + rapport (was kapot geraakt door een globale tekstvervanging bij de versie-bump in v8l). Overflow workshoprapport pagina 1 aangepakt. |
| v8n | 2026-06-13 | data v2 | _(omschrijving te bevestigen door Rob — laatst bevestigd gedeployed, commit 5623f56)_ |
| v8o | 2026-06-13 | data v2 | Workshoprapport portretpagina op `min-height:auto` (content-hoogte) zodat de voettekst niet meer naar een lege extra pagina wordt geduwd. |
| v8p | 2026-06-13 | data v2 | Evaluatie-heropzet. Invoer per formulier via 📝 Evaluatie op de workshopkaart (deelnemer-formulieren los toevoegen/verwijderen + één organisatie-evaluatie per workshop); ingevoerde formulieren als bullet-lijst op de kaart. Printbare rapporten in huisstijl: 📊 Evaluatierapport per workshop + 🏛 Gemeente-evaluatie. Evaluatie-tab met berekende cijfers (gem. cijfer, % aanbeveelt, % wil meer). Datamodel: `db.evaluaties` toegevoegd via `ensureSchema()`. **v8p vervangt v8o.** |
| v8q | 2026-06-15 | data v2 | Evaluatieformulieren voorgevuld vanuit de geselecteerde workshop (net als het deelnameformulier). Evaluatie deelnemer: Locatie + beide datums. Evaluatie organisatie: Organisatie + Contactpersoon (1e contact van de org) + Locatie + datums (op één regel). `printEvaluatieDeelnemer()` en `printEvaluatieOrganisatie()` lezen nu `sel-ws-f`. |

> Tussenliggende letters (v8c, v8d, v8g, v8h, v8k-subiteraties) waren lokale werkversies en zijn niet apart gedeployd.

## Data exports

| Versie | Bestandsnaam | Inhoud |
|--------|--------------|--------|
| data v1 | SDS_data_initieel_2026-06-08.json | Initiële import vanuit Excel |
| data v2 | SDS_data_v2_2026-06-10.json | Flyer status bijgewerkt, Shadya dag2 Charlois, SOL Feijenoord + DOCK Hoogvliet toegevoegd |

## Live versie
GitHub Pages: https://robbezemer-training.github.io/sds-beheer
Laatst bevestigd gedeployed: **v8n** (commit 5623f56) met **data v2**
Klaar om te deployen: **v8q** (`SDS_systeem_v8q_2026-06-15.html`) — evaluatieformulieren voorgevuld vanuit workshop.
(v8m–v8p stonden klaar maar deploy was nog niet bevestigd; v8q bouwt daarop voort.)

## Open punten
- **Sheets-sync voor evaluaties** — Apps Script moet het nieuwe `evaluaties`-veld aankunnen. Hangt samen met: alleen de beheerder-rol kan naar Sheets pushen (Ingrid niet) — architectuur nog te bepalen.
- **Samengevoegde JSON importeren** (eerst pull uit Sheets), controleren, pushen. Beslissen over de 2 pipeline-orgs (SOL Feijenoord / DOCK Hoogvliet, status `aangeschreven`, contactgegevens summier).
- Bij printen: certificaat-marges nog nalopen in Chrome.
