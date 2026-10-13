# Delt ledertavle (Firebase)

Uten oppsett her lagres ledertavlen bare på den enheten som spiller. For at alle som åpner lenken skal se de samme 5 beste tidene trengs en liten database. Gratisplanen (Spark) holder.

1. Gå til https://console.firebase.google.com og velg **Add project** (Google Analytics kan slås av).
2. I prosjektet: **Build → Realtime Database → Create database**. Velg region (f.eks. `europe-west1`) og start i **locked mode**.
3. Fanen **Rules**: lim inn hele innholdet i `firebase-rules.json` og trykk **Publish**.
4. Fanen **Data**: kopier URL-en øverst (f.eks. `https://havbris-xxxx-default-rtdb.europe-west1.firebasedatabase.app`).
5. Lim URL-en inn i `config.js`:
   ```js
   window.HAVBRIS_CONFIG = { leaderboardURL: "https://havbris-xxxx-default-rtdb.europe-west1.firebasedatabase.app" };
   ```
6. Commit og push til `main`. Spillet henter og lagrer da tider mot databasen.

## Regatta har egen ledertavle
Regatta-modusen lagrer i en egen gren, `regatta`, så tidene ikke blandes med «Rund bøyene». Reglene i `firebase-rules.json` dekker begge grenene. Har du allerede lagt inn reglene fra før, må du lime inn hele filen på nytt (fanen **Rules → Publish**). Ellers avvises regattatidene, og spillet viser «Fikk ikke lagret tiden». Tiden regnes fra startskuddet til mål, inkludert eventuell straff, og må være minst 2 minutter.

## Begrensninger
- Koden ligger åpent i nettleseren, så hvem som helst kan sende falske tider. Reglene begrenser kun format og verdiområde (20 s – 60 min), og at eksisterende rader ikke kan endres eller slettes.
- Sletting av uønskede rader gjøres i Firebase-konsollen (Data-fanen).
