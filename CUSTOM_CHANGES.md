# Custom Changes Documentation
# Feature: "Preis auf Anfrage" (Price on Request)

**Erstellt am:** 2026-01-16
**Branch:** claude/review-github-repo-eQDlp
**Ceres Version:** 5.0.78

---

## 🎯 Ziel der Anpassung

Artikel mit Verkaufspreis ID 8 ("Preis auf Anfrage") mit Wert 0,00 € sollen:
1. Den Text "Preis auf Anfrage" anstatt des Preises anzeigen
2. Einen "Jetzt anfragen" Button statt "In den Warenkorb" zeigen
3. Zum Kontaktformular (/kontakt) weiterleiten

---

## 📋 Modifizierte Dateien

### 1. Übersetzungsdateien

#### `/resources/lang/de/Template.properties`
**Änderung:** Neue Übersetzungsschlüssel hinzugefügt
- `itemPriceOnRequest = "Preis auf Anfrage"`
- `itemPriceOnRequestButton = "Jetzt anfragen"`

#### `/resources/lang/en/Template.properties`
**Änderung:** Neue Übersetzungsschlüssel hinzugefügt
- `itemPriceOnRequest = "Price on request"`
- `itemPriceOnRequestButton = "Request now"`

#### `/resources/lang/fr/Template.properties`
**Änderung:** Neue Übersetzungsschlüssel hinzugefügt
- `itemPriceOnRequest = "Prix sur demande"`
- `itemPriceOnRequestButton = "Demander maintenant"`

#### `/resources/lang/nl/Template.properties`
**Änderung:** Neue Übersetzungsschlüssel hinzugefügt
- `itemPriceOnRequest = "Prijs op aanvraag"`
- `itemPriceOnRequestButton = "Nu aanvragen"`

#### `/resources/lang/pl/Template.properties`
**Änderung:** Neue Übersetzungsschlüssel hinzugefügt
- `itemPriceOnRequest = "Cena na zapytanie"`
- `itemPriceOnRequestButton = "Zapytaj teraz"`

---

### 2. Vue-Komponenten

#### `/resources/js/src/app/components/item/ItemPrice.vue`
**Änderung:** Preisanzeige-Logik für Einzelartikelseite
- **Status:** ✅ ABGESCHLOSSEN
- **Zeilen geändert:** 1-80 (Template), 102-112 (Script)
- **Details:**
  - **Template-Änderungen:**
    - Zeilen 3-14: Neuer "Preis auf Anfrage" Block mit Button zu `/kontakt`
    - Zeilen 16-45: Bestehender Preis-Block in `<template v-else>` gewrappt
    - Zeilen 47, 57, 64: `!isPriceOnRequest` Bedingung hinzugefügt zu properties/lowest-price/base-price
  - **Script-Änderungen:**
    - Zeilen 108-112: Neue computed property `isPriceOnRequest()`
    - Logik: `return currentVariation.prices.default && currentVariation.prices.default.price.value === 0`
  - **Verhalten:**
    - Bei Preis = 0,00 €: Zeigt "Preis auf Anfrage" + "Jetzt anfragen" Button (→ /kontakt)
    - Bei normalem Preis: Zeigt reguläre Preisanzeige
    - Preis-Details (lowest-price, base-price, properties) werden bei "Preis auf Anfrage" ausgeblendet

#### `/resources/js/src/app/components/itemList/CategoryItem.vue`
**Änderung:** Preisanzeige-Logik für Kategorieansicht
- **Status:** ✅ ABGESCHLOSSEN
- **Zeilen geändert:** 62-94 (Template), 276-280 (Script), 99, 103 (Conditions)
- **Details:**
  - **Template-Änderungen:**
    - Zeilen 62-67: Neuer "Preis auf Anfrage" Block (nur Text, kein Button in Kategorieansicht)
    - Zeilen 69-94: Bestehender Preis-Block in `<template v-else>` gewrappt
    - Zeile 99: `!isPriceOnRequest` zu lowest-price Bedingung hinzugefügt
    - Zeile 103: `!isPriceOnRequest` zu unit-price Bedingung hinzugefügt
  - **Script-Änderungen:**
    - Zeilen 276-280: Neue computed property `isPriceOnRequest()`
    - Logik: `return item.prices.default && item.prices.default.price.value === 0`
  - **Verhalten:**
    - Bei Preis = 0,00 €: Zeigt "Preis auf Anfrage" (ohne Button)
    - Bei normalem Preis: Zeigt reguläre Preisanzeige
    - Lowest-price und Unit-price werden bei "Preis auf Anfrage" ausgeblendet

#### `/resources/js/src/app/components/basket/AddToBasket.vue`
**Änderung:** Button-Logik für Kontaktformular-Weiterleitung
- **Status:** ✅ ABGESCHLOSSEN
- **Zeilen geändert:** 28-61 (Desktop), 78-92 (Mobile)
- **Details:**
  - **Desktop-Version (Zeilen 28-61):**
    - Zeilen 28-37: Neuer "Preis auf Anfrage" Button (`<a>` Tag zu /kontakt)
    - Zeilen 39-61: Bestehende Buttons in `v-else-if` Struktur geändert
    - Icon: fa-envelope (Brief-Symbol)
    - Button-Text: Übersetzungsschlüssel "itemPriceOnRequestButton"
  - **Mobile-Version (Zeilen 78-92):**
    - Zeilen 78-82: Neuer "Preis auf Anfrage" Button (Mobile)
    - Zeilen 84-92: Bestehende Buttons in `v-else-if` Struktur geändert
  - **Verhalten:**
    - Bei `!hasPrice`: Zeigt "Jetzt anfragen" Button, verlinkt zu `/kontakt`
    - Bei `hasPrice`: Zeigt normalen "In den Warenkorb" Button
    - Funktioniert auf Desktop und Mobile
    - Das `hasPrice` prop wird von SingleItem.vue übergeben (Filter: hasItemDefaultPrice)

---

## 🔄 Implementierungs-Logik

### Bedingung für "Preis auf Anfrage":
```javascript
// Prüfung in Vue-Komponenten:
if (currentVariation.prices.default && currentVariation.prices.default.price.value === 0) {
    // Zeige "Preis auf Anfrage"
    // Zeige "Jetzt anfragen" Button statt "In den Warenkorb"
    // Button verlinkt zu: /kontakt
}
```

### Verkaufspreis-Konfiguration:
- **Verkaufspreis ID:** 8
- **Name:** "Preis auf Anfrage"
- **Wert:** 0,00 €
- **Mandant:** Alle

---

## 🔧 Build-Prozess

Nach allen Änderungen muss das Projekt neu gebaut werden:
```bash
npm install
npm run build
```

Dies erstellt die kompilierten Dateien in `/resources/js/dist/`

---

## 📦 Commit-Historie

### Commit 1: Feature "Preis auf Anfrage" vollständig implementiert
**Datum:** 2026-01-16
**Geänderte Dateien:** 8 Dateien

**Übersetzungen (5 Dateien):**
- `/resources/lang/de/Template.properties` - Zeilen 453-454
- `/resources/lang/en/Template.properties` - Zeilen 453-454
- `/resources/lang/fr/Template.properties` - Zeilen 443-444
- `/resources/lang/nl/Template.properties` - Zeilen 443-444
- `/resources/lang/pl/Template.properties` - Zeilen 443-444

**Vue-Komponenten (3 Dateien):**
- `/resources/js/src/app/components/item/ItemPrice.vue` - Template & computed property
- `/resources/js/src/app/components/itemList/CategoryItem.vue` - Template & computed property
- `/resources/js/src/app/components/basket/AddToBasket.vue` - Template (Desktop & Mobile)

**Dokumentation (1 Datei):**
- `/CUSTOM_CHANGES.md` - Vollständige Änderungsdokumentation

**Zusammenfassung:**
Implementiert "Preis auf Anfrage" Feature für Artikel mit Verkaufspreis ID 8 (Preis = 0,00 €). Zeigt "Preis auf Anfrage" Text und "Jetzt anfragen" Button, der zum Kontaktformular (/kontakt) weiterleitet. Funktioniert auf Einzelartikelseite, Kategorieansicht, Desktop und Mobile.

### Commit 2: Plugin-Name eindeutig gemacht
**Datum:** 2026-01-16
**Geänderte Dateien:** 1 Datei

**plugin.json Änderungen:**
- **name:** "Ceres" → "YEEQCeresCustom" (eindeutiger interner Name)
- **marketplaceName:** "YEEQ Ceres - Preis auf Anfrage" (DE) / "YEEQ Ceres - Price on Request" (EN)
- **description:** Erweitert mit spezifischer Beschreibung
- **author:** "CyrussOne / YEEQ Shop"
- **shortDescription:** Angepasst für YEEQ Custom Template
- **email:** info@yeeq.de
- **keywords:** Erweitert um "YEEQ", "custom", "price on request", "preis auf anfrage"

**Zweck:** Eindeutige Identifizierung des Plugins in PlentyOne, keine Verwechslung mit Original plentyShop LTS

### Commit 3: Build erstellt
- PENDING (wird bei Bedarf durchgeführt)

---

## 🔍 Troubleshooting: "Plugin CfourCustomCssJs requires 'Ceres'"

### Problem:
Trotz korrekter Konfiguration (name: "Ceres", version: "5.0.79") erscheint die Fehlermeldung weiterhin bei der Plugin-Bereitstellung.

### Schritt-für-Schritt-Lösung:

#### 1. Plugin-Set vollständig prüfen
Gehen Sie zu **Plugins » Plugin-Set-Übersicht** und prüfen Sie:
- [ ] Ist **plentyShop LTS** (Original) wirklich vollständig gelöscht (nicht nur deaktiviert)?
- [ ] Ist **YEEQ Ceres - Preis auf Anfrage** im Plugin-Set vorhanden?
- [ ] Welche Version wird bei **YEEQ Ceres** angezeigt?

**So löschen Sie das Original-Plugin vollständig:**
1. Plugins » Plugin-Set-Übersicht öffnen
2. Ihr aktives Plugin-Set auswählen
3. Bei "plentyShop LTS" auf das **Mülleimer-Symbol** klicken (NICHT Deaktivieren!)
4. Löschung bestätigen

#### 2. Plugin komplett neu installieren

**A) Altes Plugin entfernen:**
1. Plugins » Plugin-Set-Übersicht
2. "YEEQ Ceres - Preis auf Anfrage" über das Mülleimer-Symbol löschen
3. Speichern und warten bis Änderung übernommen wurde

**B) Neues Plugin installieren:**
1. Plugins » Git
2. Bei Ihrem Repository den **"Installieren" Button** klicken
3. **WICHTIG:** Branch auswählen: `claude/review-github-repo-eQDlp`
4. Plugin-Set auswählen (Ihr aktives Set)
5. Installation abschließen

**C) Plugin aktivieren:**
1. Plugins » Plugin-Set-Übersicht
2. Ihr Plugin-Set öffnen
3. "YEEQ Ceres - Preis auf Anfrage" aktivieren (Häkchen setzen)
4. Position prüfen: Template-Plugins sollten vor Theme-Plugins stehen
5. Speichern

#### 3. Plugin-Reihenfolge prüfen
Die Reihenfolge im Plugin-Set ist wichtig:
```
1. IO (Basis-Plugin)
2. YEEQ Ceres - Preis auf Anfrage (Template)
3. CfourCustomCssJs (Theme/Erweiterung)
4. Weitere Plugins...
```

**So ändern Sie die Reihenfolge:**
1. Plugins » Plugin-Set-Übersicht » Ihr Set
2. Plugins per Drag & Drop verschieben
3. Speichern

#### 4. Plugin-Bereitstellung mit detaillierter Prüfung

**Vor der Bereitstellung:**
1. Plugins » Plugin-Set-Übersicht
2. Prüfen Sie die angezeigten Plugin-Namen:
   - Es sollte **KEIN** "plentyShop LTS" mehr vorhanden sein
   - "YEEQ Ceres - Preis auf Anfrage" sollte Version **5.0.79** zeigen
   - "CfourCustomCssJs" sollte aktiv sein

**Bereitstellung starten:**
1. Plugins » Plugin-Set-Übersicht
2. Bei Ihrem Plugin-Set auf **"Bereitstellen"** klicken
3. **Deployment-Log genau beobachten**

**Mögliche Fehlerursachen im Log:**
- "Plugin nicht gefunden" → Git-Installation prüfen
- "Version nicht gefunden" → Branch-Auswahl prüfen
- "Abhängigkeit fehlt" → IO-Plugin prüfen

#### 5. Cache leeren (falls Problem weiterhin besteht)

**A) Browser-Cache:**
1. Browser-Cache vollständig leeren
2. Inkognito-/Privater Modus für Test verwenden

**B) PlentyOne-Cache:**
1. System » Systemeinstellungen » Dienste » Cache
2. "Backend-Cache leeren" ausführen
3. "Template-Cache leeren" ausführen

#### 6. Alternative: Plugin-Set neu erstellen

Falls alles andere nicht hilft:
1. Neues Plugin-Set erstellen: Plugins » Plugin-Set-Übersicht » **Neues Set erstellen**
2. Name: z.B. "YEEQ Shop Custom"
3. Basis-Plugins installieren (IO, etc.)
4. YEEQ Ceres aus Git installieren (Branch: `claude/review-github-repo-eQDlp`)
5. CfourCustomCssJs und andere Plugins hinzufügen
6. Plugin-Set dem Mandanten zuweisen
7. Bereitstellen

#### 7. Versions-Informationen verifizieren

Prüfen Sie im Git-Repository (GitHub):
- Branch: `claude/review-github-repo-eQDlp`
- Letzter Commit: `781ebf6` (oder neuer)
- plugin.json Zeile 2: `"version": "5.0.79"`
- plugin.json Zeile 3: `"name": "Ceres"`

**Falls die Version nicht stimmt:**
1. Plugins » Git » Repository neu laden (Refresh)
2. Plugin erneut installieren

#### 8. Support-Informationen sammeln

Falls der Fehler weiterhin auftritt, sammeln Sie diese Informationen:
- [ ] Screenshot der Plugin-Set-Übersicht (alle Plugins sichtbar)
- [ ] Screenshot des Deployment-Logs (vollständige Fehlermeldung)
- [ ] Welche Version wird bei "YEEQ Ceres" in der Plugin-Übersicht angezeigt?
- [ ] Ist das Original "plentyShop LTS" wirklich weg (Screenshot)?
- [ ] Branch-Auswahl beim Git-Plugin (Screenshot)

### Häufigste Ursachen:
1. **Original-Plugin nicht gelöscht** - Nur deaktiviert statt gelöscht
2. **Falscher Branch** - "stable" statt "claude/review-github-repo-eQDlp"
3. **Cache-Problem** - PlentyOne nutzt alte Plugin-Informationen
4. **Falsche Reihenfolge** - Template-Plugin steht nach Theme-Plugin

---

## ⚠️ Rollback-Anleitung

### Falls etwas schief geht:

1. **Alle Änderungen rückgängig machen:**
   ```bash
   git reset --hard HEAD~[ANZAHL_COMMITS]
   ```

2. **Nur bestimmte Dateien zurücksetzen:**
   ```bash
   git checkout HEAD -- [DATEI_PFAD]
   ```

3. **Zu einem bestimmten Commit zurück:**
   ```bash
   git log  # Finde den Commit-Hash
   git reset --hard [COMMIT_HASH]
   ```

### Original-Dateien wiederherstellen:

Alle Originaldateien sind im Git-Repository gesichert auf dem letzten Commit vor unseren Änderungen:
- **Commit-Hash:** `e325df1fda14ea3452fd9091d55642869723c2c8`
- **Commit-Nachricht:** "Merge pull request #3761 from plentymarkets/fix/alt_text_from_webspace_images_for_manufacturer_images"

Um wiederherzustellen:
```bash
git checkout [COMMIT_HASH] -- [DATEI_PFAD]
```

---

## 🔄 Update-Strategie bei Ceres-Updates

### Vor dem Update:
1. Sichern Sie Ihre Änderungen:
   ```bash
   git checkout -b backup-preis-auf-anfrage-[DATUM]
   ```

### Nach dem Ceres-Update:
1. Prüfen Sie, welche Dateien von CERES geändert wurden:
   ```bash
   git diff origin/stable..HEAD
   ```

2. Vergleichen Sie mit den in diesem Dokument aufgelisteten Dateien

3. Falls Konflikte:
   - Nutzen Sie dieses Dokument als Referenz
   - Wenden Sie die Änderungen manuell erneut an
   - Testen Sie gründlich

---

## 📝 Test-Checkliste

Nach der Implementierung testen:

- [ ] Artikeldetailseite: "Preis auf Anfrage" wird angezeigt
- [ ] Kategorieansicht: "Preis auf Anfrage" wird angezeigt
- [ ] Button "Jetzt anfragen" erscheint statt "In den Warenkorb"
- [ ] Button verlinkt korrekt zu /kontakt
- [ ] Alle Sprachen funktionieren (DE, EN, FR, NL, PL)
- [ ] Responsive Design auf mobilen Geräten
- [ ] Normale Artikel (mit Preis) funktionieren weiterhin
- [ ] Build-Prozess läuft ohne Fehler

---

## 🛠️ Technische Details

### Framework-Versionen:
- Vue.js: 2.6.12
- Vuex: 3.3.0
- Bootstrap: 4.4.1
- Webpack: 4.43.0

### Browser-Kompatibilität:
- Getestet in: [WIRD NACH TESTS EINGETRAGEN]
- Bekannte Probleme: [KEINE/ODER LISTE]

---

## 📞 Kontakt & Support

Bei Problemen oder Fragen:
1. Prüfen Sie dieses Dokument
2. Prüfen Sie die Git-Commits
3. Konsultieren Sie die plentymarkets-Dokumentation

---

**Letzte Aktualisierung:** 2026-01-16 (Feature vollständig implementiert)
**Nächste geplante Überprüfung:** Bei nächstem Ceres-Update
**Status:** ✅ IMPLEMENTIERUNG ABGESCHLOSSEN - Bereit für Build und Testing
