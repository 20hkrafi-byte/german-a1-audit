# 03 — Form Trainer Engine & Complete Data
## Source: `german-exam-html` — Script 3 IIFE (Lines 446–528)

---

## 1. Form Trainer Overview

The Form Trainer is a **monkey-patched IIFE** (Script 3) that intercepts the `render()` function when `section === 'formLab'`. It is the second fully OPEN section (alongside Model Test 1).

**Entry point:** Sidebar button `🧾 Form Trainer` → `go('formLab', this)` → `render()` → intercepted by patched `render` → `formLabPage()`

---

## 2. IIFE Helper Functions

### 2.1 `esc(s)` — HTML Escape

```javascript
function esc(s) {
  return String(s || '').replace(/[&<>]/g, function(c) {
    return {'&':'&amp;','<':'&lt;','>':'&gt;'}[c];
  });
}
```

### 2.2 `idSafe(s)` — ID Sanitizer

```javascript
function idSafe(s) {
  return String(s || '').replace(/[^a-zA-Z0-9_]/g, '_');
}
```

### 2.3 `sayTxt(t)` — TTS Text Cleaner

```javascript
function sayTxt(t) {
  return String(t || '').replace(/`/g, '').replace(/<[^>]*>/g, ' ').replace(/\s+/g, ' ').trim();
}
```

### 2.4 `slaFormSay(t)` — Form Audio

```javascript
window.slaFormSay = function(t) {
  try { if (typeof window.speak === 'function') return window.speak(sayTxt(t), 'de-DE'); }
  catch(e) {}
  return false;
};
```

### 2.5 `slaFormToggle(id)` — Toggle Answer Reveal

```javascript
window.slaFormToggle = function(id) {
  var e = document.getElementById(id);
  if (e) { e.style.display = e.style.display === 'block' ? 'none' : 'block'; }
};
```

**Line:** 448–452
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 3. `FORM_RULES` — 8 Golden Rules (Verbatim)

| # | Rule (English) | Detail | Bangla |
|---|---|---|---|
| 1 | Copy, do not invent | Names, dates, cities, streets and numbers must come from the text. Do not create your own data. | নাম, তারিখ, শহর, রাস্তা, নম্বর text থেকে copy করুন; নিজের মতো বানাবেন না। |
| 2 | Separate Vorname and Familienname | German forms often ask first name and surname separately. In "Maria Alvarez", Maria is Vorname, Alvarez is Familienname. | Vorname = প্রথম নাম, Familienname = পদবি। |
| 3 | Keep names unchanged | Do not translate or Bangla-spell German/Spanish/Arabic names in the form field. | Form field-এ নাম translate করবেন না। |
| 4 | Date format | If the text gives 14.03.1998, write exactly 14.03.1998 unless the form asks another format. | তারিখের format বদলাবেন না, যদি form না চায়। |
| 5 | Address split | Straße, Hausnummer, PLZ, Wohnort can be separate boxes. Read carefully. | Straße, house number, postal code, city আলাদা হতে পারে। |
| 6 | Language fields | Muttersprache means mother tongue. Andere Sprache/Fremdsprache means other/foreign language. | Muttersprache = মাতৃভাষা; Fremdsprache = অন্য ভাষা। |
| 7 | Course time | Vormittag, Nachmittag, Abend are copied as time preferences. | Vormittag/Nachmittag/Abend course time হিসেবে copy করুন। |
| 8 | Signature area | Unterschrift means signature. Ort/Datum means place and date. | Unterschrift = signature; Ort/Datum = স্থান/তারিখ। |

**Line:** 454–463
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 4. `FIELD_BANK` — 24 Form Field Definitions (Verbatim)

| # | German Field | English | Bangla | Example |
|---|---|---|---|---|
| 1 | Familienname | surname / last name | পদবি | Khan / Müller / Alvarez |
| 2 | Vorname | first name | প্রথম নাম | Rahim / Anna / Maria |
| 3 | Geburtsdatum | date of birth | জন্মতারিখ | 12.05.2000 |
| 4 | Geburtsort | place of birth | জন্মস্থান | Dhaka / Berlin |
| 5 | Geburtsland | country of birth | জন্মদেশ | Bangladesch |
| 6 | Staatsangehörigkeit | nationality | জাতীয়তা | bangladesch / spanisch |
| 7 | Adresse | address | ঠিকানা | Gartenstraße 26 |
| 8 | Straße | street | রাস্তার নাম | Gartenstraße |
| 9 | Hausnummer | house number | বাড়ি নম্বর | 26 |
| 10 | PLZ | postal code | পোস্ট কোড | 10115 |
| 11 | Wohnort | city of residence | বাসস্থান শহর | Berlin |
| 12 | Telefonnummer | phone number | ফোন নম্বর | 017... |
| 13 | E-Mail | email | ই-মেইল | rahim@mail.com |
| 14 | Muttersprache | mother tongue | মাতৃভাষা | Bangla |
| 15 | Fremdsprache | foreign language | বিদেশি ভাষা | Englisch |
| 16 | Beruf | job/profession | পেশা | Student / Verkäuferin |
| 17 | Kursart | course type | কোর্সের ধরন | Deutsch A1 |
| 18 | Kurszeit | course time | কোর্স সময় | Vormittag |
| 19 | Anreise | arrival | আগমন | 12. Juni |
| 20 | Abreise | departure | প্রস্থান | 15. Juni |
| 21 | Einzelzimmer | single room | একজনের রুম | 1 Einzelzimmer |
| 22 | Doppelzimmer | double room | দুইজনের রুম | 1 Doppelzimmer |
| 23 | Personenzahl | number of people | মানুষের সংখ্যা | 2 Personen |
| 24 | Unterschrift | signature | স্বাক্ষর | Rahim Khan |

**Line:** 465–467
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 5. `FORMS` — 12 Full Form Exercises (Verbatim)

### Form 1: Deutschkurs Anmeldung (Course Registration)

**Tag:** Course registration
**Source Text:** Ali Karim kommt aus Bangladesch. Er ist am 12.05.2000 in Dhaka geboren. Er wohnt jetzt in Berlin, Sonnenallee 45, 12045 Berlin. Seine Muttersprache ist Bangla. Er spricht auch Englisch. Er möchte einen Deutschkurs A1 am Abend besuchen.

| Field | Answer |
|---|---|
| Familienname | Karim |
| Vorname | Ali |
| Geburtsdatum | 12.05.2000 |
| Geburtsort | Dhaka |
| Wohnort | Berlin |
| Straße / Hausnummer | Sonnenallee 45 |
| PLZ | 12045 |
| Muttersprache | Bangla |
| Weitere Sprache | Englisch |
| Kurs | Deutsch A1 |
| Kurszeit | Abend |

### Form 2: Bibliotheksausweis (Library Card)

**Tag:** Library card
**Source Text:** Sara Ahmed möchte einen Bibliotheksausweis. Sie wohnt in München, Blumenstraße 18. Die Postleitzahl ist 80331. Sie ist Studentin. Sie ist am 03.11.1999 in Kairo geboren. Ihre Telefonnummer ist 0176 44332211.

| Field | Answer |
|---|---|
| Familienname | Ahmed |
| Vorname | Sara |
| Geburtsdatum | 03.11.1999 |
| Geburtsort | Kairo |
| Beruf | Studentin |
| Straße / Hausnummer | Blumenstraße 18 |
| PLZ | 80331 |
| Wohnort | München |
| Telefonnummer | 0176 44332211 |

### Form 3: Hotel Anmeldung (Hotel Form)

**Tag:** Hotel form
**Source Text:** Herr David Brown reserviert ein Einzelzimmer im Hotel Sonne. Er kommt am 14.07. an und fährt am 17.07. ab. Er kommt aus Kanada. Seine Passnummer ist K908771. Er möchte Frühstück im Hotel.

| Field | Answer |
|---|---|
| Familienname | Brown |
| Vorname | David |
| Zimmer | Einzelzimmer |
| Hotel | Hotel Sonne |
| Anreise | 14.07. |
| Abreise | 17.07. |
| Land | Kanada |
| Passnummer | K908771 |
| Frühstück | ja |

### Form 4: Reisebüro Anmeldung (Travel Agency)

**Tag:** Travel agency
**Source Text:** Fatima Rahman möchte im Reisebüro eine Reise nach Hamburg buchen. Sie fährt mit dem Zug. Sie reist am Freitag, den 08.09., und bleibt zwei Nächte. Sie braucht ein Doppelzimmer für zwei Personen.

| Field | Answer |
|---|---|
| Familienname | Rahman |
| Vorname | Fatima |
| Reiseziel | Hamburg |
| Verkehrsmittel | Zug |
| Reisedatum | 08.09. |
| Tag | Freitag |
| Nächte | 2 |
| Zimmer | Doppelzimmer |
| Personenzahl | 2 Personen |

### Form 5: Arztpraxis Formular (Doctor Form)

**Tag:** Doctor form
**Source Text:** Pavel Novak ist krank. Er hat Fieber und Husten. Er möchte am Montag einen Termin bei Dr. Klein. Er ist am 22.02.1995 geboren. Seine Krankenkasse heißt AOK. Seine Telefonnummer ist 0157 90909090.

| Field | Answer |
|---|---|
| Familienname | Novak |
| Vorname | Pavel |
| Geburtsdatum | 22.02.1995 |
| Beschwerden | Fieber und Husten |
| Wunschtermin | Montag |
| Arzt | Dr. Klein |
| Krankenkasse | AOK |
| Telefonnummer | 0157 90909090 |

### Form 6: Wohnungsbesichtigung (Apartment Viewing)

**Tag:** Apartment viewing
**Source Text:** Nina Fischer sucht eine Wohnung in Leipzig. Sie interessiert sich für die 2-Zimmer-Wohnung in der Mozartstraße 7. Sie möchte die Wohnung am Dienstag um 16 Uhr besichtigen. Ihre E-Mail-Adresse ist nina.fischer@mail.de.

| Field | Answer |
|---|---|
| Familienname | Fischer |
| Vorname | Nina |
| Wohnort gesucht | Leipzig |
| Wohnung | 2-Zimmer-Wohnung |
| Adresse der Wohnung | Mozartstraße 7 |
| Besichtigungstag | Dienstag |
| Uhrzeit | 16 Uhr |
| E-Mail | nina.fischer@mail.de |

### Form 7: VHS Kurskarte (Adult Education Course)

**Tag:** Adult education course
**Source Text:** Carlos Mendes möchte an der Volkshochschule einen Deutschkurs besuchen. Er braucht den Kurs am Vormittag. Er spricht Portugiesisch und ein bisschen Englisch. Er wohnt in Köln, Ringstraße 12.

| Field | Answer |
|---|---|
| Familienname | Mendes |
| Vorname | Carlos |
| Institution | Volkshochschule |
| Kurs | Deutschkurs |
| Kurszeit | Vormittag |
| Muttersprache | Portugiesisch |
| Weitere Sprache | Englisch |
| Wohnort | Köln |
| Straße / Hausnummer | Ringstraße 12 |

### Form 8: Sportverein Anmeldung (Sports Club)

**Tag:** Sports club
**Source Text:** Lisa Müller möchte im Sportverein Tennis spielen. Sie ist 19 Jahre alt und hat am Wochenende Zeit. Sie wohnt in Stuttgart, Parkweg 5. Ihre Telefonnummer ist 0711 222333.

| Field | Answer |
|---|---|
| Familienname | Müller |
| Vorname | Lisa |
| Alter | 19 Jahre |
| Verein | Sportverein |
| Sportart | Tennis |
| Zeit | Wochenende |
| Wohnort | Stuttgart |
| Straße / Hausnummer | Parkweg 5 |
| Telefonnummer | 0711 222333 |

### Form 9: Sprachschule Formular (Language School)

**Tag:** Language school
**Source Text:** Yvonne Legrand aus Frankreich möchte Deutsch lernen. Sie ist am 17.04.1993 in Lyon geboren. Sie spricht Französisch und Englisch. Sie möchte den Kurs am Nachmittag besuchen.

| Field | Answer |
|---|---|
| Familienname | Legrand |
| Vorname | Yvonne |
| Land | Frankreich |
| Geburtsdatum | 17.04.1993 |
| Geburtsort | Lyon |
| Muttersprache | Französisch |
| Weitere Sprache | Englisch |
| Kurs | Deutsch |
| Kurszeit | Nachmittag |

### Form 10: Post Paketkarte (Post Office)

**Tag:** Post office
**Source Text:** Rahim Khan möchte ein Paket nach Bonn schicken. Der Empfänger heißt Max Schneider. Die Adresse ist Marktstraße 9, 53111 Bonn. Das Paket wiegt 2 Kilo. Rahim wohnt in Dhaka und schreibt als Absender: Rahim Khan.

| Field | Answer |
|---|---|
| Absender Name | Rahim Khan |
| Empfänger Name | Max Schneider |
| Empfänger Straße | Marktstraße 9 |
| Empfänger PLZ | 53111 |
| Empfänger Ort | Bonn |
| Gewicht | 2 Kilo |
| Sendung | Paket |

### Form 11: Bank Kontoeröffnung (Bank Form)

**Tag:** Bank form
**Source Text:** Amina Begum möchte ein Konto eröffnen. Sie wohnt in Frankfurt, Mainzer Landstraße 40. Sie ist Verkäuferin. Sie ist am 05.06.1997 geboren. Ihre E-Mail-Adresse ist amina.begum@mail.de.

| Field | Answer |
|---|---|
| Familienname | Begum |
| Vorname | Amina |
| Anliegen | Konto eröffnen |
| Beruf | Verkäuferin |
| Geburtsdatum | 05.06.1997 |
| Wohnort | Frankfurt |
| Straße / Hausnummer | Mainzer Landstraße 40 |
| E-Mail | amina.begum@mail.de |

### Form 12: Schule Entschuldigung (School Absence)

**Tag:** School absence
**Source Text:** Der Schüler Tim Bauer kann am Donnerstag nicht zur Schule kommen. Er ist krank und hat Kopfschmerzen. Seine Mutter heißt Anna Bauer. Die Klasse ist A1-2.

| Field | Answer |
|---|---|
| Schülername | Tim Bauer |
| Tag | Donnerstag |
| Grund | krank / Kopfschmerzen |
| Mutter | Anna Bauer |
| Klasse | A1-2 |

**Line:** 469–482
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 6. Mini Quizzes — 8 Fast Field Recognition Drills

```javascript
const qs = [
  ['Maria Hoffmann, geboren am 10.10.2001 in Wien.', 'Vorname', 'Maria'],
  ['Maria Hoffmann, geboren am 10.10.2001 in Wien.', 'Familienname', 'Hoffmann'],
  ['Maria Hoffmann, geboren am 10.10.2001 in Wien.', 'Geburtsort', 'Wien'],
  ['Karim wohnt in Berlin, Hauptstraße 33, 10115 Berlin.', 'Straße / Hausnummer', 'Hauptstraße 33'],
  ['Karim wohnt in Berlin, Hauptstraße 33, 10115 Berlin.', 'PLZ', '10115'],
  ['Sie spricht Arabisch und Deutsch.', 'Muttersprache / Sprache', 'Arabisch / Deutsch'],
  ['Anreise: Montag, 12.06. Abreise: Freitag, 16.06.', 'Anreise', '12.06.'],
  ['Anreise: Montag, 12.06. Abreise: Freitag, 16.06.', 'Abreise', '16.06.']
];
```

**UI:** Each drill renders as a card with: source text, field name prompt, text input, Answer reveal button, Play button.

**Line:** 493–495
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 7. Error Lab — 6 Common Mistake Rows

| Wrong | Correct | Why | Bangla |
|---|---|---|---|
| Vorname: Khan / Familienname: Rahim | Vorname: Rahim / Familienname: Khan | Name order was reversed. | নাম উল্টো হয়েছে। |
| Geburtsort: 12.05.2000 | Geburtsdatum: 12.05.2000 | Date was put into place field. | তারিখ ভুল field-এ গেছে। |
| Wohnort: Sonnenallee 45 | Straße/Hausnummer: Sonnenallee 45; Wohnort: Berlin | Street and city are different fields. | রাস্তা ও শহর আলাদা। |
| Muttersprache: Englisch, but text says Bangla | Muttersprache: Bangla; Weitere Sprache: Englisch | Mother tongue is not the same as other language. | মাতৃভাষা ও অন্য ভাষা আলাদা। |
| Kurszeit: morning | Kurszeit: Vormittag | Use German word from task/form. | German word ব্যবহার করুন। |
| Familienname translated into Bangla | Keep the original spelling | Do not translate names in German forms. | নাম translate করবেন না। |

**Line:** 497–499
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 8. `formLabPage()` — Full Page Assembly

```javascript
window.formLabPage = function() {
  return  slaFormHero
        + goldenRulesSection (rulesHtml())
        + fieldBankSection   (bankHtml())
        + pdfBookSection     (9 topic pills)
        + miniQuizzes()
        + "12 Full Form Exercises" header + FORMS.map(formExercise).join('')
        + errorLab()
        + finalExamChecklist;
};
```

**Page sections in order:**
1. **Hero Banner** — "Form Trainer Mega Lab — Goethe A1 Formular Mastery"
2. **Golden Rules** — 8 rules in 4-column grid
3. **Field Bank** — 24-field table with Play button
4. **PDF Book Preview** — 9 topic pills (Persönliche Daten, Formular ausfüllen, Kurs Anmeldung, Hotel Anmeldung, Reisebüro, Wohnung, Arztpraxis, Bibliothek, Bank / Post)
5. **Mini Quizzes** — 8 fast drills
6. **12 Form Exercises** — Each with source text, input fields, answer reveal, audio buttons
7. **Error Lab** — 6 common mistake rows
8. **Final Exam Checklist** — 6-item self-review list

**Line:** 502–511
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 9. Render Monkey-Patch

```javascript
const oldRender = window.render || (typeof render === 'function' ? render : null);
if (oldRender && !window.SLA_FORM_MEGA_RENDER_PATCHED) {
  window.SLA_FORM_MEGA_RENDER_PATCHED = true;
  window.render = render = function() {
    if (section === 'formLab') {
      if (typeof updateProgress === 'function') updateProgress();
      var m = document.getElementById('main') || window.main;
      if (m) { m.innerHTML = window.formLabPage(); }
      if (typeof postRender === 'function') postRender();
      return;
    }
    return oldRender();
  };
}
// Auto-render if already on formLab
if (typeof section !== 'undefined' && section === 'formLab' && typeof render === 'function') render();
```

**Pattern:** Saves reference to original `render()`, replaces it with a wrapper that:
1. If `section === 'formLab'` → renders Form Trainer page
2. Otherwise → delegates to original `render()`

**Flag:** `SLA_FORM_MEGA_RENDER_PATCHED` prevents double-patching if script runs twice.

**Line:** 513–528
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 10. Form Exercise Renderer — `formExercise(f, idx)`

Each form generates:
1. **Tag pills** (e.g., "Form 3", "Hotel form")
2. **Topic heading** (e.g., "Hotel Anmeldung")
3. **Audio button** — speaks source text + all answers
4. **Source text** in `.slaFormText` container
5. **Input grid** — 2-column grid of `<label>` + `<input placeholder="write here">` for each field
6. **3 action buttons:** "Show Filled Form" (reveals answer), "Play Source Text", "Play Answers"
7. **Answer panel** (hidden by default) — shows all field:answer pairs + Bangla method note

> [!NOTE]
> There is **NO auto-validation** of form inputs. The user types freely, then clicks "Show Filled Form" to self-check against the correct answers. This is deliberate — it mirrors the Goethe A1 Schreiben Teil 1 format where students fill a printed form.

**Line:** 486–492
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 11. Form Trainer Statistics

| Metric | Count |
|---|---|
| Golden Rules | 8 |
| Field Definitions | 24 |
| Full Form Exercises | 12 |
| Mini Quiz Drills | 8 |
| Error Lab Rows | 6 |
| Final Checklist Items | 6 |
| Unique Characters | Ali Karim, Sara Ahmed, David Brown, Fatima Rahman, Pavel Novak, Nina Fischer, Carlos Mendes, Lisa Müller, Yvonne Legrand, Rahim Khan, Amina Begum, Tim Bauer |
| Unique Form Types | Course registration, Library card, Hotel, Travel agency, Doctor, Apartment, VHS, Sports club, Language school, Post office, Bank, School absence |
| Total Form Fields (sum) | 11+9+9+9+8+8+9+9+9+7+8+5 = **111 fields** |

**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

*End of file — 03-FORM-TRAINER-ENGINE-AND-DATA.md*
