# 08 — Extracted Sample Data
## German A1 App #02: "Sound, Pronunciation & Vocabulary Lab"

---

## 1. German Directions — Complete Chapter 1 Data `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

### 1.1 Card Data Table

| # | Icon | German | Phonetic | English | Note | Example (DE) | Example (EN) |
|:-:|:----:|--------|----------|---------|------|-------------|-------------|
| 1 | ➡️ | rechts | rekhts | right | Relative direction word. | Geh nach rechts. | Go right. |
| 2 | ⬅️ | links | links | left | Relative direction word. | Geh nach links. | Go left. |
| 3 | ⬆️ | geradeaus | ge-RAH-de-ows | forward / straight ahead | Use this to continue in the same direction. | Geh geradeaus. | Go straight ahead. |
| 4 | ⬇️ | zurück | tsu-RÜK | back / backward | Use this when someone must go back. | Geh zurück. | Go back. |

### 1.2 Raw Data (JavaScript)

```javascript
const chapters = [{
  name: '1. Basic directions',
  mode: true,
  cards: [
    ['➡️','rechts','rekhts','right','Relative direction word.','Geh nach rechts.','Go right.'],
    ['⬅️','links','links','left','Relative direction word.','Geh nach links.','Go left.'],
    ['⬆️','geradeaus','ge-RAH-de-ows','forward / straight ahead',
     'Use this to continue in the same direction.','Geh geradeaus.','Go straight ahead.'],
    ['⬇️','zurück','tsu-RÜK','back / backward',
     'Use this when someone must go back.','Geh zurück.','Go back.']
  ]
}];
```

### 1.3 Locked Chapter Names (11 total)

| # | Chapter Name |
|:-:|-------------|
| 1 | Basic directions *(open)* |
| 2 | Compass directions *(locked)* |
| 3 | Turn commands *(locked)* |
| 4 | Position words *(locked)* |
| 5 | Near and far *(locked)* |
| 6 | Direction questions *(locked)* |
| 7 | Mini routes *(locked)* |
| 8 | City places *(locked)* |
| 9 | Transport directions *(locked)* |
| 10 | Classroom commands *(locked)* |
| 11 | Review sentences *(locked)* |

### 1.4 Compass Mode Markers

| Position | Relative Mode | Absolute Mode |
|----------|:------------:|:-------------:|
| Top | geradeaus | Norden |
| Bottom | zurück | Süden |
| Left | links | Westen |
| Right | rechts | Osten |
| Center | du | du |

### 1.5 Chip Words Rendered

**Relative mode:** rechts, links, geradeaus, zurück
**Absolute mode:** Norden, Süden, Osten, Westen

---

## 2. French Opposite Commands — Complete Chapter 1 Data `[VERIFIED FROM SHARED CODEBASE: french-opposite-html]`

### 2.1 Card Data Table

| # | Icon | French | Phonetic | English | Bengali | Animation | Pair |
|:-:|:----:|--------|----------|---------|---------|-----------|:----:|
| 1 | 🫴 | Viens ici ! | vyen ee-see | Come here! | এখানে আসো! | move-center | A |
| 2 | 👋 | Va là-bas ! | vah lah-bah | Go there! | ওখানে যাও! | move-away | A |
| 3 | ⬅️ | Va à gauche ! | vah ah gosh | Go left! | বামে যাও! | move-left | B |
| 4 | ➡️ | Va à droite ! | vah ah drwaht | Go right! | ডানে যাও! | move-right | B |

### 2.2 Grammar Rule (Chapter 1)

> The French **tu** imperative normally removes the subject pronoun. **venir → Viens !** and **aller → Va !** are irregular. With directions use **ici**, **là-bas**, **à gauche** and **à droite**.

### 2.3 Locked Chapter Names (10 total)

| # | Chapter | Icon |
|:-:|---------|:----:|
| 1 | Movement Commands *(open)* | 🚶 |
| 2 | Body Position *(locked)* | 🧍 |
| 3 | Door and Window *(locked)* | 🚪 |
| 4 | Book Commands *(locked)* | 📖 |
| 5 | Pen Commands *(locked)* | ✏️ |
| 6 | Classroom Movement *(locked)* | 🏫 |
| 7 | Speed and Voice *(locked)* | 🗣️ |
| 8 | Light and Computer *(locked)* | 💡 |
| 9 | Hands and Body *(locked)* | 🤲 |
| 10 | Action Control *(locked)* | ⏯️ |

---

## 3. Chinese HSK1 — Complete Starter Sentences `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

| # | Chinese | Pinyin | English | Bengali |
|:-:|---------|--------|---------|---------|
| 1 | 你好 | nǐ hǎo | hello | হ্যালো |
| 2 | 谢谢 | xièxie | thank you | ধন্যবাদ |
| 3 | 对不起 | duìbuqǐ | sorry | দুঃখিত |
| 4 | 没关系 | méiguānxi | no problem | সমস্যা নেই |
| 5 | 你叫什么名字？ | Nǐ jiào shénme míngzi? | What is your name? | আপনার নাম কী? |
| 6 | 我叫 Karim。 | Wǒ jiào Karim. | My name is Karim. | আমার নাম করিম। |
| 7 | 你是哪国人？ | Nǐ shì nǎ guó rén? | Which country are you from? | আপনি কোন দেশের মানুষ? |
| 8 | 我是孟加拉国人。 | Wǒ shì Mèngjiālāguó rén. | I am Bangladeshi. | আমি বাংলাদেশি। |

### 3.1 Example Question Card

```
Chinese: 你喝水吗？
Pinyin:  Nǐ hē shuǐ ma?
English: Do you drink water?
Bengali: তুমি কি পানি পান কর?
```

### 3.2 HSK1 Sections (9 total)

| # | Icon | Section | Status |
|:-:|:----:|---------|:------:|
| 1 | 🌱 | Novice Start | Open |
| 2 | 🖼️ | Visual + Dot Trace Lab | Locked |
| 3 | 🎧 | Listening Paper 1–20 | Locked |
| 4 | 📖 | Reading Paper 21–40 | Locked |
| 5 | 🧱 | Paper Vocabulary | Locked |
| 6 | 🧠 | HSK 1 Grammar | Locked |
| 7 | 🔊 | Audio Script Bank | Locked |
| 8 | ⏱️ | Timed Mock Mode | Locked |
| 9 | 🧑‍🏫 | Teacher Notes | Locked |

---

## 4. Turkish Daily Commands — Chapter 1 Scenarios (First 3) `[VERIFIED FROM SHARED CODEBASE: turkish-daily-html]`

### 4.1 Scenario 1: "Şekeri getir" (Bring the sugar)

**Scene:** Kitchen
**Props:** Dolap 🗄️ (72,29) · Şeker 🧂 (72,43) · Masa 🍽️ (25,67)

| Step | Turkish | English | Bengali | Action | W4W |
|:----:|---------|---------|---------|--------|-----|
| 1 | Mutfağa git. | Go to the kitchen. | রান্নাঘরে যাও। | move | Mutfağa=to the kitchen / git=go |
| 2 | Dolabı aç. | Open the cupboard. | আলমারিটি খোলো। | open | Dolabı=the cupboard / aç=open |
| 3 | Şekeri masaya getir. | Bring the sugar to the table. | চিনিটা টেবিলে নিয়ে আসো। | bring | Şekeri=the sugar / masaya=to the table / getir=bring |

### 4.2 Scenario 2: "Bir fincan çay hazırla" (Make a cup of tea)

**Scene:** Kitchen
**Props:** Su ısıtıcısı 🫖 (69,55) · Fincan ☕ (31,62) · Çay 🫙 (73,33) · Musluk 🚰 (47,47)

| Step | Turkish | English | Bengali |
|:----:|---------|---------|---------|
| 1 | Su ısıtıcısını suyla doldur. | Fill the kettle with water. | কেটলিতে পানি ভরো। |
| 2 | Su ısıtıcısını çalıştır. | Switch on the kettle. | কেটলি চালু করো। |
| 3 | Çayı fincana koy. | Put the tea in the cup. | কাপে চা দাও। |
| 4 | Sıcak suyu fincana dök. | Pour the hot water into the cup. | কাপে গরম পানি ঢালো। |

### 4.3 Scenario 3: "Kahvaltıyı hazırla" (Prepare breakfast)

**Scene:** Kitchen
**Props:** Buzdolabı 🧊 (78,37) · Ekmek 🍞 (31,47) · Peynir 🧀 (78,53) · Tabak 🍽️ (29,65)

| Step | Turkish | English | Bengali |
|:----:|---------|---------|---------|
| 1 | Buzdolabını aç. | Open the fridge. | ফ্রিজ খোলো। |
| 2 | Peyniri çıkar. | Take out the cheese. | চিজ বের করো। |
| 3 | Ekmeği ve peyniri tabağa koy. | Put the bread and cheese on the plate. | পাউরুটি আর চিজ প্লেটে রাখো। |

### 4.4 Chapter 1 Grammar Rule (EN)

> **Turkish command order:** the verb normally comes last. A destination takes the **dative -(y)e/-(y)a**: *mutfağa git, masaya getir*. A specific object commonly takes the **accusative -(y)i/ı/u/ü**: *dolabı aç, şekeri getir*. The everyday singular command is usually the bare verb stem: *git, aç, getir*.

### 4.5 Chapter 1 Grammar Rule (BN)

> **তুর্কি command-এ verb সাধারণত শেষে থাকে।** গন্তব্যে **-(y)e/-(y)a** dative লাগে: *mutfağa git, masaya getir*। নির্দিষ্ট object-এ **-(y)i/ı/u/ü** accusative লাগে: *dolabı aç, şekeri getir*। দৈনন্দিন singular command সাধারণত verb stem: *git, aç, getir*।

### 4.6 Locked Chapters (10 total)

| # | Turkish Title | English Title | Bengali Title | Icon |
|:-:|--------------|--------------|--------------|:----:|
| 1 | Mutfak ve Yemekler | Kitchen & Meals | রান্নাঘর ও খাবার | 🍽️ |
| 2 | Sabah ve Yatak Odası | Morning & Bedroom | সকাল ও শোবার ঘর | 🛏️ |
| 3 | Banyo ve Kişisel Bakım | Bathroom & Personal Care | বাথরুম ও ব্যক্তিগত পরিচর্যা | 🪥 |
| 4 | Temizlik ve Çamaşır | Cleaning & Laundry | পরিষ্কার-পরিচ্ছন্নতা ও কাপড় ধোয়া | 🧹 |
| 5 | Evden Çıkma | Leaving Home | *(locked)* | 🚪 |
| 6 | Alışveriş ve Günlük İşler | Shopping & Errands | *(locked)* | 🛒 |
| 7 | Sokak ve Ulaşım | Street & Transport | *(locked)* | 🚌 |
| 8 | İş ve Ders | Work & Study | *(locked)* | 💼 |
| 9 | Salon ve Aile | Living Room & Family | *(locked)* | 🛋️ |
| 10 | Güvenlik ve Küçük Acil Durumlar | Safety & Small Emergencies | *(locked)* | 🚨 |

---

## 5. A1 Pronunciation & Listening Syllabus `[VERIFIED LIVE]`

### 5.1 A1 Level

| # | Topic (English) | IPA / Notation |
|:-:|-----------------|---------------|
| 1 | ä / ö / ü and vowel length | /ɛː/, /øː/, /yː/ vs short variants |
| 2 | ich-sound vs Bach-sound for ch | /ç/ vs /x/ |
| 3 | z = /ts/, w = /v/, sch, initial sp / st | /ts/, /v/, /ʃ/, /ʃp/, /ʃt/ |
| 4 | Final devoicing | /d/→/t/, /b/→/p/, /g/→/k/ word-finally |
| 5 | Word stress and clear syllable rhythm | Stress patterns, Trochaic tendency |

### 5.2 A2 Level (For Reference)

| # | Topic |
|:-:|-------|
| 1 | Connected speech in common phrases |
| 2 | Sentence stress for new vs known information |
| 3 | Long compounds and compound stress |
| 4 | Clear case/adjective endings |
| 5 | Natural but controlled A2 listening |

### 5.3 B1 Level (For Reference)

| # | Topic |
|:-:|-------|
| 1 | Natural sentence rhythm |
| 2 | Contrastive stress |
| 3 | Compound nouns and professional vocabulary |
| 4 | Intonation for agreement and disagreement |
| 5 | Longer dialogues, interviews and reports |

---

## 6. A1 Grammar & Structure Syllabus `[VERIFIED LIVE]`

Extracted from `/german` page:

| # | Grammar Topic |
|:-:|--------------|
| 1 | Personal pronouns; sein and haben; regular and common irregular present verbs |
| 2 | Definite and indefinite articles: der / die / das, ein / eine |
| 3 | Nominative and accusative foundations |
| 4 | Negation with nicht and kein |
| 5 | V2 word order in statements; yes/no and W-questions |
| 6 | Modal verbs: können, müssen, wollen, dürfen, möchten |
| 7 | Separable verbs and the basic sentence bracket |
| 8 | Possessives: mein, dein, sein, ihr, unser |
| 9 | Plural patterns and noun gender awareness |
| 10 | Core place/time prepositions and first dative patterns |

---

## 7. A1 Can-Do Output Outcomes `[VERIFIED LIVE]`

| # | Outcome |
|:-:|---------|
| 1 | Introduce yourself |
| 2 | Ask and answer everyday questions |
| 3 | Order food and shop |
| 4 | Describe routine, family and home |
| 5 | Write short messages and simple forms |
| 6 | Understand short signs, notices and basic audio |

---

## 8. CATALOG Chapter Structures for German Apps `[VERIFIED FROM SHARED CODEBASE]`

### 8.1 German Directions — 11 Chapters

Chapters listed in §1.3 above.

### 8.2 German A2 Complete Mastery — 7 Chapters

| # | Chapter Name |
|:-:|-------------|
| 1 | Kapitel 1 — Perfekt, Alltag, Erfahrungen und Erlebnisse |
| 2 | Kapitel 2 — Nebensätze: weil, dass, wenn, obwohl und damit |
| 3 | Kapitel 3 — Wechselpräpositionen: Wo? Wohin? Dativ oder Akkusativ |
| 4 | Kapitel 4 — Modalverben, reflexive Verben und Alltagspflichten |
| 5 | Kapitel 5 — Adjektive, Komparativ, Superlativ und Beschreibungen |
| 6 | Kapitel 6 — A2 Prüfungstraining: Schreiben, Sprechen, Lesen und Alltag |
| 7 | Kapitel 7 — Arbeit, Bewerbung, Ausbildung und Bürokommunikation |

### 8.3 German A1 Exam Prep — 14 Chapters

| # | Chapter | Meaning | Access |
|:-:|---------|---------|:------:|
| 1 | Model Test 1 | Personal Information, Shopping, Short Signs | Open |
| 2–13 | Model Tests 2–13 | Various exam topics | Locked |
| 14 | Form Trainer | Full form practice | Open |

---

*File generated: 2026-11-24 · All data extracted from live sources*
