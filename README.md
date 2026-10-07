# Häxans hus - release

## Publicera på GitHub Pages

Publicera innehållet i den här mappen som webbplatsens rot. `index.html` är startsidan; lämna `assets/` och övriga filer på samma relativa sökvägar. Intro- och avslutningsvideorna strömmas från Dropbox och ingår inte som lokala MP4-filer i releasen.

## Firebase Realtime Database

Spelet använder Firebase Authentication med anonym inloggning och Realtime Database. Anonym autentisering måste vara aktiverad i Firebase Console.

Kopiera reglerna från `firebase-database.rules.json` till Realtime Database → Rules i Firebase Console och publicera dem. Reglerna låter autentiserade spelare läsa och skriva spellobbynas data. Återanslutningskoder ligger separat under `reconnectCodes`: spelare kan slå upp en kod de redan känner till, men kan inte läsa upp hela listan. Admin-e-postadressen som anges i reglerna får läsa listan.

### Admin och återanslutningskoder

1. Aktivera leverantören **E-post/lösenord** under Firebase Console → Authentication → Sign-in method.
2. Skapa ett admin-konto under Authentication → Users.
3. Kontrollera att admin-kontot använder `adrian.nessler@teliacompany.com` och publicera reglerna från `firebase-database.rules.json` i Realtime Database. Regeln är låst till den adressen; utan att publicera den kan admin logga in men inte läsa kodlistan.
4. Spelare får en personlig återanslutningskod när de ansluter. De kan använda koden från startsidan för att återta sin plats även efter att sidan eller webbläsaren stängts.
5. Skriv `Admin` i lösenordsfältet på startsidan och tryck **Fortsätt**. Logga sedan in med admin-kontot för att visa eller kopiera aktiva spelares koder.

Spelarens återanslutningskod visas i väntrummet och som en knapp under ljudinställningarna under spelets gång. Klicka på koden för att kopiera den.

Koden är en slumpmässig, 12 tecken lång hemlig nyckel: den som har den kan ta över spelarens plats. Dela den bara med rätt spelare. Databasreglerna nekar åtkomst till kodlistan för andra spelare; direkt åtkomst till ett enskilt kodobjekt kräver att spelaren redan känner till den koden.

Speldata lagras per lobby under `rooms/{lobbyId}` med grenarna `lobby`, `world`, `players`, `events` och `spiritVision`. Firebase-konfigurationen ligger i `game.html`; webb-API-nyckeln är klientkonfiguration och ska skyddas med databasreglerna.

Delad lobby- och speldata läses och skrivs bara via Firebase. Vid frånkoppling blockeras spelet tills Firebase har återanslutit och laddat in aktuella lobbysnapshots; den delade datan sparas inte som reserv i `localStorage`.

Miljöbilder laddas när scenen behövs och närliggande scener förladdas i bakgrunden. Scenbyte väntar på att den nya bakgrunden har laddats och avkodats, med en tidsgräns så att spelet inte fastnar vid ett långsamt asset. Ljud förladdas inte i bulk utan hämtas när uppspelning behövs.
