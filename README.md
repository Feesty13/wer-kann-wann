# Wer kann wann

Kleine Web-App für Aushilfen: Jeder trägt ein, wann er nicht arbeiten kann. Der Planer sieht die ganze Woche auf einen Blick.

- **Wochenübersicht** mit Kalenderwoche, Montag bis Samstag, „X von Y da“ pro Tag. Auf dem Handy erscheint sie als Tagesliste.
- **Meine Termine**: einmalig, als Zeitraum oder jede Woche (Schule, Uni, Training). Ganztags, vormittags, nachmittags oder eigene Uhrzeit. Als „kann nicht“ oder „lieber nicht“.
- **Team** (nur Planer): Neue Leute freischalten, Planer ernennen, Personen entfernen.
- Lässt sich auf dem Handy wie eine App auf den Startbildschirm legen.

Solange in `config.js` keine Firebase-Daten stehen, läuft die Seite als **Demo mit Beispieldaten**. Änderungen bleiben dann nur im jeweiligen Browser.

---

## Echten Betrieb einschalten (ca. 20 Minuten)

Du brauchst ein Google-Konto. Alles ist kostenlos (Firebase „Spark“-Tarif, keine Kreditkarte nötig).

### 1. Firebase-Projekt anlegen
1. Öffne https://console.firebase.google.com und klicke auf **Projekt erstellen**.
2. Name: `wer-kann-wann` (oder ähnlich). Google Analytics kannst du **ausschalten**.

### 2. Login einschalten
1. Links im Menü: **Build → Authentication → Jetzt starten**.
2. Reiter **Anmeldemethode** → **E-Mail-Adresse/Passwort** → aktivieren → Speichern.
   (Nur den ersten Schalter, „E-Mail-Link“ bleibt aus.)
3. Reiter **Einstellungen** → **Autorisierte Domains** → **Domain hinzufügen** → die Adresse eurer Seite ohne `https://` und ohne Pfad eintragen, z. B. `deinname.github.io`.

### 3. Datenbank anlegen
1. Links: **Build → Firestore Database → Datenbank erstellen**.
2. Standort: **europe-west3 (Frankfurt)**.
3. **Im Produktionsmodus starten** wählen.
4. Wenn die Datenbank fertig ist: Reiter **Regeln** öffnen, alles darin löschen und den kompletten Inhalt der Datei [`firestore.rules`](firestore.rules) aus diesem Repository einfügen → **Veröffentlichen**.

### 4. Web-App registrieren und Konfiguration eintragen
1. Oben links auf das **Zahnrad → Projekteinstellungen**.
2. Unten bei „Meine Apps“ auf das Symbol **`</>`** (Web) klicken.
3. App-Name: `Wer kann wann`. „Firebase Hosting“ **nicht** anhaken. → **App registrieren**.
4. Es erscheint ein Code-Block mit `const firebaseConfig = { ... }`. Diese Werte in [`config.js`](config.js) eintragen, sodass dort steht:

   ```js
   export const firebaseConfig = {
     apiKey: "...",
     authDomain: "...",
     projectId: "...",
     storageBucket: "...",
     messagingSenderId: "...",
     appId: "...",
   };
   ```

   Das geht direkt auf GitHub: Datei `config.js` öffnen → Stift-Symbol → einfügen → **Commit changes**. Nach ein bis zwei Minuten ist die Seite aktualisiert.

   Die Werte sind keine Passwörter. Die Daten schützen die Regeln aus Schritt 3.

### 5. Loslegen
1. Seite öffnen → **Registrieren** → Name eingeben.
2. Oben erscheint „Noch kein Planer festgelegt“ → **Ich bin der Planer**. (Das geht nur ein einziges Mal. Mach es also direkt selbst.)
3. Link in die WhatsApp-Gruppe schicken. Wer sich registriert, erscheint bei dir im Reiter **Team** unter „Wartet auf Freischaltung“.
4. Wenn der Chef mitmacht: Er registriert sich, setzt beim Namen den Haken „Ich mache nur den Plan“, du schaltest ihn frei und klickst **Zum Planer machen**.

### Wer darf was

| | Aushilfe (freigeschaltet) | Planer |
|---|---|---|
| Eigene Termine eintragen/löschen | ✓ | ✓ |
| Wochenübersicht sehen | ✓ | ✓ |
| Termine anderer löschen | – | ✓ |
| Leute freischalten / entfernen | – | ✓ |
| Planer ernennen | – | ✓ |

Wer noch nicht freigeschaltet ist, kann schon eigene Termine eintragen, sieht aber die Übersicht der anderen noch nicht. Diese Regeln setzt die Datenbank durch (`firestore.rules`), nicht nur die Seite.

### Passwort vergessen
Auf der Anmeldeseite gibt es „Passwort vergessen?“. Firebase schickt dann einen Link per E-Mail. Er landet manchmal im Spam-Ordner.

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | Die komplette App |
| `config.js` | Firebase-Konfiguration (`null` = Demo) |
| `firestore.rules` | Zugriffsregeln für die Datenbank |
| `manifest.webmanifest`, `icons/` | Für „Zum Startbildschirm hinzufügen“ |
