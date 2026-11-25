# 07 — Complete Verb Data Extraction: German A1 App #05

**Audit Date:** 2026-11-24  
**Target:** App #05 — Verb Conjugation & Action Drill  
**Primary Source:** `italian-verbs-html` bundle — inline JSON data from line 74

---

## 1. Data Extraction Summary

| Data Source | Items | Status |
|-------------|:-----:|:------:|
| `VERBS` array (full row data) | 1 verb (essere) | ✅ Fully extracted |
| `verbCatalog` array (catalog entries) | 20 verbs | ✅ Fully extracted |
| Remaining 19 verb row data | 19 verbs | 🔴 Locked |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 2. Full `essere` Verb Data (Complete 6-Row Paradigm)

This is the **only fully unlocked verb** in the demo. Every field is extracted verbatim.

### 2.0 Verb Header

```json
{
  "verb": "essere",
  "en": "to be",
  "bn": "হওয়া/থাকা"
}
```

### 2.1 Row 1 — `io` (1st person singular)

| Field | Value |
|-------|-------|
| **pronoun** | `io` |
| **form** | `sono` |
| **sentence** | `Io sono Karim.` |
| **en** | `I am Karim.` |
| **bn** | `আমি করিম।` |
| **w4w** | `Io = I \| sono = am \| Karim = Karim` |

### 2.2 Row 2 — `tu` (2nd person singular)

| Field | Value |
|-------|-------|
| **pronoun** | `tu` |
| **form** | `sei` |
| **sentence** | `Tu sei uno studente.` |
| **en** | `You are a student.` |
| **bn** | `তুমি একজন ছাত্র।` |
| **w4w** | `Tu = you \| sei = are \| uno studente = a student` |

### 2.3 Row 3 — `lui/lei` (3rd person singular)

| Field | Value |
|-------|-------|
| **pronoun** | `lui/lei` |
| **form** | `è` |
| **sentence** | `Lei è a casa.` |
| **en** | `She is at home.` |
| **bn** | `সে বাসায় আছে।` |
| **w4w** | `Lei = she \| è = is \| a casa = at home` |

### 2.4 Row 4 — `noi` (1st person plural)

| Field | Value |
|-------|-------|
| **pronoun** | `noi` |
| **form** | `siamo` |
| **sentence** | `Noi siamo in classe.` |
| **en** | `We are in class.` |
| **bn** | `আমরা ক্লাসে আছি।` |
| **w4w** | `Noi = we \| siamo = are \| in classe = in class` |

### 2.5 Row 5 — `voi` (2nd person plural)

| Field | Value |
|-------|-------|
| **pronoun** | `voi` |
| **form** | `siete` |
| **sentence** | `Voi siete pronti.` |
| **en** | `You all are ready.` |
| **bn** | `তোমরা প্রস্তুত।` |
| **w4w** | `Voi = you all \| siete = are \| pronti = ready` |

### 2.6 Row 6 — `loro` (3rd person plural)

| Field | Value |
|-------|-------|
| **pronoun** | `loro` |
| **form** | `sono` |
| **sentence** | `Loro sono in Italia.` |
| **en** | `They are in Italy.` |
| **bn** | `তারা ইতালিতে আছে।` |
| **w4w** | `Loro = they \| sono = are \| in Italia = in Italy` |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 74, verbatim extraction.

---

## 3. Full Conjugation Chart: `essere` — Presente

| Pronoun | Form | Sentence | English | Bangla | Word for Word |
|---------|------|----------|---------|--------|---------------|
| io | sono | Io sono Karim. | I am Karim. | আমি করিম। | Io = I \| sono = am \| Karim = Karim |
| tu | sei | Tu sei uno studente. | You are a student. | তুমি একজন ছাত্র। | Tu = you \| sei = are \| uno studente = a student |
| lui/lei | è | Lei è a casa. | She is at home. | সে বাসায় আছে। | Lei = she \| è = is \| a casa = at home |
| noi | siamo | Noi siamo in classe. | We are in class. | আমরা ক্লাসে আছি। | Noi = we \| siamo = are \| in classe = in class |
| voi | siete | Voi siete pronti. | You all are ready. | তোমরা প্রস্তুত। | Voi = you all \| siete = are \| pronti = ready |
| loro | sono | Loro sono in Italia. | They are in Italy. | তারা ইতালিতে আছে। | Loro = they \| sono = are \| in Italia = in Italy |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 4. Complete `verbCatalog` — 20 Verbs

All 20 verbs from the `verbCatalog` array, extracted verbatim:

| # | Verb | English | Bengali | Demo Status |
|:-:|------|---------|---------|:-----------:|
| 1 | essere | to be | হওয়া/থাকা | 🟢 Unlocked |
| 2 | avere | to have | থাকা/পাওয়া | 🔴 Locked |
| 3 | chiamarsi | to be called / to call oneself | নাম হওয়া | 🔴 Locked |
| 4 | abitare | to live | বসবাস করা | 🔴 Locked |
| 5 | parlare | to speak | কথা বলা | 🔴 Locked |
| 6 | studiare | to study | পড়াশোনা করা | 🔴 Locked |
| 7 | andare | to go | যাওয়া | 🔴 Locked |
| 8 | venire | to come | আসা | 🔴 Locked |
| 9 | mangiare | to eat | খাওয়া | 🔴 Locked |
| 10 | bere | to drink | পান করা | 🔴 Locked |
| 11 | comprare | to buy | কেনা | 🔴 Locked |
| 12 | pagare | to pay | দাম দেওয়া | 🔴 Locked |
| 13 | capire | to understand | বুঝতে পারা | 🔴 Locked |
| 14 | leggere | to read | পড়া | 🔴 Locked |
| 15 | scrivere | to write | লেখা | 🔴 Locked |
| 16 | fare | to do / make | করা/বানানো | 🔴 Locked |
| 17 | volere | to want | চাওয়া | 🔴 Locked |
| 18 | potere | can / to be able to | পারা | 🔴 Locked |
| 19 | dovere | must / to have to | অবশ্যই করা/করতে হবে | 🔴 Locked |
| 20 | prendere | to take | নেওয়া | 🔴 Locked |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 74, verbatim extraction.

---

## 5. Verb Classification by Type

| Category | Verbs | Count |
|----------|-------|:-----:|
| Irregular | essere, avere, andare, venire, bere, fare, volere, potere, dovere | 9 |
| Regular -are | chiamarsi, abitare, parlare, studiare, mangiare, comprare, pagare | 7 |
| Regular -ere | leggere, scrivere, prendere | 3 |
| Regular -ire | capire | 1 |

**Pedagogical ordering:** The catalog follows a logical learning sequence:
1. **Core copula** (essere = to be)
2. **Core auxiliary** (avere = to have)
3. **Self-introduction** (chiamarsi = to be called)
4. **Daily life** (abitare, parlare, studiare)
5. **Movement** (andare, venire)
6. **Food** (mangiare, bere)
7. **Commerce** (comprare, pagare)
8. **Cognition** (capire, leggere, scrivere)
9. **General action** (fare)
10. **Modal verbs** (volere, potere, dovere)
11. **Utility** (prendere = to take)

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 6. Data Design Observations

### 6.1 Sentence Design Principles

Based on the 6 `essere` sentences:

| Principle | Example |
|-----------|---------|
| **A1-appropriate vocabulary** | "Io sono Karim." (name introduction) |
| **Real-world context** | "Lei è a casa." (at home) |
| **Varied scenarios per pronoun** | Each pronoun gets a different situational sentence |
| **Bengali name inclusion** | "Karim" — culturally relevant to Bangladeshi learners |
| **Minimal sentence length** | 3–5 words per sentence |

### 6.2 W4W Design Principles

| Principle | Example |
|-----------|---------|
| **Multi-word glosses allowed** | `uno studente = a student` (not word-by-word atomic) |
| **Preposition phrases grouped** | `a casa = at home`, `in classe = in class`, `in Italia = in Italy` |
| **Proper nouns self-glossed** | `Karim = Karim` |
| **Function words included** | Every word in the sentence appears in the W4W gloss |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 7. `fillSelect()` — How the Catalog Populates the Dropdown

```javascript
function fillSelect() {
  const sel = $("verbSelect");
  sel.innerHTML = verbCatalog.map((v, i) =>
    `<option value="${i}" ${i > 0 ? 'disabled' : ''}>` +
    `${i+1}. ${v.verb}${i > 0 ? ' - locked' : ' — ' + v.en + ' / ' + v.bn}` +
    `</option>`
  ).join('');

  sel.onchange = () => {
    stopAll(false);
    index = 0;
    sel.value = '0';
    updateCount();
    showVerb(0);
  };
}
```

**Dropdown rendering:**
- Item 1: `"1. essere — to be / হওয়া/থাকা"` (enabled)
- Item 2: `"2. avere - locked"` (disabled)
- Items 3–20: `"N. verb - locked"` (disabled)

**Selection behavior:** Even if a user somehow selects a locked verb, the `onchange` handler resets to index 0. This is a UI-level lock — the actual verb data for indices 1–19 simply doesn't exist in the `VERBS` array.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 79.

---

## 8. Cross-Reference: German A2 Verb Data in `german-a2-html`

The `german-a2-html` bundle contains verb conjugation content within its grammar sections (specifically Perfekt tense with haben/sein). However, it uses the **Complete Mastery template**, not the Verb Conjugation Lab template, so the data format is different:

| Aspect | App #05 (Verb Lab) | A2 Grammar Verbs |
|--------|:------------------:|:----------------:|
| Template | Verb Conjugation Lab | Complete Mastery |
| Data format | `VerbData` with `rows` | Embedded in `.rulebox`/`.examplebox` HTML |
| W4W field | ✅ Per row | ❌ Not present |
| Chart | ✅ 6-column grid | ❌ Inline examples only |
| Tense | Präsens (A1) | Perfekt (A2) |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html, german-a2-html]`

---

*End of file — 07-COMPLETE-VERB-DATA-EXTRACTION.md*
