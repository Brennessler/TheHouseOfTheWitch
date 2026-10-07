# Häxans hus - release

## Publicera på GitHub Pages

Publicera innehållet i den här mappen som webbplatsens rot. `index.html` är startsidan; lämna `assets/` och övriga filer på samma relativa sökvägar. Intro- och avslutningsvideorna strömmas från Dropbox och ingår inte som lokala MP4-filer i releasen.

## Firebase Realtime Database

Spelet använder Firebase Authentication med anonym inloggning och Realtime Database. Anonym autentisering måste vara aktiverad i Firebase Console.

Kopiera reglerna från `firebase-database.rules.json` till Realtime Database → Rules i Firebase Console och publicera dem. Reglerna kräver autentiserad åtkomst men innebär att alla anonyma spelare kan läsa och skriva spellobbynas data. Spara inte känslig information där.

Speldata lagras per lobby under `rooms/{lobbyId}` med grenarna `lobby`, `world`, `players`, `events` och `spiritVision`. Firebase-konfigurationen ligger i `game.html`; webb-API-nyckeln är klientkonfiguration och ska skyddas med databasreglerna.
