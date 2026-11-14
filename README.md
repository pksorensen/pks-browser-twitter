# pks-browser-twitter

Browser-opskrifter til X, kørt **lokalt** af [Agentics browser-tray](https://agentics.dk/products/pks-agent-browser)
på en profil du selv har logget ind i. Intet login og ingen cookie forlader maskinen.

**[Åbn opskrifterne på agentics.dk →](https://agentics.dk/browser/recipes/pksorensen/pks-browser-twitter)** — siden læser dette repo
direkte og viser for hver opskrift hvad trinene gør, hvilke værter den rører, og
om den kan køre uovervåget. Alt på siden er låst til én commit, også filen du
henter, så et push imellem ikke kan bytte opskriften ud under dig.

## Opskrifter

| Fil | Hvad den gør | Profil | Status |
| --- | --- | --- | --- |
| [`recipes/x-bogmaerker.json`](recipes/x-bogmaerker.json) | Høster dine bogmærker — kun nyt siden sidst | `x` | kører |

## Installér en opskrift

1. Gå til **[opskriftssiden](https://agentics.dk/browser/recipes/pksorensen/pks-browser-twitter)** og tryk *Installér i trayen*. Trayen henter opskriften
   og viser en kvittering: hvilken commit den kom fra, hvilke værter den rører, om den
   handler eller kun læser, og hvad aftrykket er. Siger du ja, ligger den i mappen og
   står i menuen med det samme. **Kør nu** er det samme, plus at den kører bagefter.
2. Sker der ingenting, kender maskinen ikke `agentics-browser://` endnu — start trayen
   én gang, og prøv igen. Den melder sig som den der håndterer adressen ved hver start.

I hånden, hvis du vil se filen først:

1. Tryk *Hent opskriften* på [opskriftssiden](https://agentics.dk/browser/recipes/pksorensen/pks-browser-twitter) — eller direkte fra
   [GitHub](https://raw.githubusercontent.com/pksorensen/pks-browser-twitter/main/recipes/x-bogmaerker.json), højreklik → Gem som.
2. Læg den i `%USERPROFILE%\.agentics\browser-tray\recipes\`
3. Højreklik på tray-ikonet. Menuen bygges forfra hver gang den åbnes, så opskriften er der med det samme — ingen genstart.

**Behold filnavnet.** Trayen nøgler vandmærket (`x-bogmaerker.seen.json`) på filnavnet, så en omdøbning starter høsten forfra.

Trayen beder altid om din godkendelse af trinnene. Det er med vilje: en opskrift kan nå en
browser hvor du er logget ind. For en fil du selv har lagt i mappen hænger godkendelsen på
trinnenes aftryk — ændrer filen sig, bliver du spurgt igen. Kommer opskriften fra en knap på
en webside, bliver du spurgt **hver gang**, og det ja bliver aldrig en stående tilladelse til
at køre den uovervåget. Et klik på en webside er ikke det samme som et klik i din egen menu.

## Krav

```powershell
irm https://agentics.dk/install/pks-agent-browser-tray.ps1 | iex
```

Dertil en profil der heder `x` i trayens `settings.json`, og at du har logget ind i den én gang via menuen (*Log ind i profil 'x'…*).

## Kør fra kommandolinjen

Menuen er den understøttede vej — den viser dialoger og fejl. Kommandolinjen findes også:

```powershell
$tray = "$env:LOCALAPPDATA\Agentics\bin\pks-agent-browser-tray.exe"
& $tray recipes                                          # hvad ligger der, og kan det køres
& $tray run --name x-bogmaerker *> run.log; cat run.log  # kør én opskrift
```

Omdirigeringen er ikke pynt: trayen er bygget som `WinExe` og har ingen konsol, så uden `*> fil` forsvinder både resultatet og fejlen.

## Hvad du skal vide om denne opskrift

- **X har omdøbt Bogmærker til "History".** `x.com/i/bookmarks` omdirigerer nu til `x.com/i/history`, hvor Bogmærker er landingsfanen. Derfor venter opskriften på et *bogmærkekort* (`assert`) frem for på adressen — en adresseventetid brækkede 7/11-2026.
- **Selektoren er stadig under tilpasning.** Den nuværende form skærer `/analytics`- og `/photo/`-links væk, men trækker fortsat citerede opslag med. Kandidaten er `article a[href*='/status/']:has(time)`, som giver præcis én nøgle pr. kort; den mangler en måling på en rigtig side.
- **Fremdriften er musehjul, ikke tastetryk.** `"key": "j"` er fjernet — programmatisk scroll standsede 498 px fra bunden.
- **X serverer ingenting til en browser der ikke er logget ind.** Selektorer kan derfor ikke måles fra en tom profil.

## Rettelser

Push til `main`. Der er ingen udgivelse at vente på: hent filen igen, eller tryk **Kør nu** på opskriftssiden — den læser `main` og er låst til den commit du ser.
