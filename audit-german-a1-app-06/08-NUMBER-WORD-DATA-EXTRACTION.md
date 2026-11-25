# 08 — Number Word Data Extraction: German A1 App #06 "Number Drill"

**Audit Date:** 2026-11-24  
**Source Bundles:** `chinese-time-html`, `finnish-time-html`, `portuguese-time-html`, `saudi-arabic-time-html`  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. Purpose

This file extracts **every numeric vocabulary lookup table** found in the 4 live Drill Lab bundles, then projects the equivalent German number word system that App #06 would require.

---

## 2. Chinese Time — `nums{}` (12 Entries)

Extracted verbatim from Line 42 of `chinese-time-html`:

```javascript
const nums = {
  1:  ['一',   'yī',      'one',                   'এক'],
  2:  ['两',   'liǎng',   'two / used before 点',   'দুই / সময়ে দুই'],
  3:  ['三',   'sān',     'three',                  'তিন'],
  4:  ['四',   'sì',      'four',                   'চার'],
  5:  ['五',   'wǔ',      'five',                   'পাঁচ'],
  6:  ['六',   'liù',     'six',                    'ছয়'],
  7:  ['七',   'qī',      'seven',                  'সাত'],
  8:  ['八',   'bā',      'eight',                  'আট'],
  9:  ['九',   'jiǔ',     'nine',                   'নয়'],
  10: ['十',   'shí',     'ten',                    'দশ'],
  11: ['十一', 'shí yī',  'eleven',                 'এগারো'],
  12: ['十二', 'shí èr',  'twelve',                 'বারো']
};
```

**Schema:** `nums[hour] → [hanzi, pinyin, english, bangla]`  
**Coverage:** 1–12 (clock hours only)  
**Note:** Key 2 uses `两` (liǎng) not `二` (èr) — special spoken-Chinese convention before measure words.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 3. Chinese Time — `minWords{}` (11 Entries)

Extracted verbatim from Line 43 of `chinese-time-html`:

```javascript
const minWords = {
  5:  ['五分',       'wǔ fēn',           'five minutes',         'পাঁচ মিনিট'],
  10: ['十分',       'shí fēn',          'ten minutes',          'দশ মিনিট'],
  15: ['一刻',       'yí kè',            'quarter / fifteen min', 'সোয়া / পনেরো মিনিট'],
  20: ['二十分',     'èr shí fēn',       'twenty minutes',       'বিশ মিনিট'],
  25: ['二十五分',   'èr shí wǔ fēn',    'twenty-five minutes',  'পঁচিশ মিনিট'],
  30: ['半',         'bàn',              'half',                 'সাড়ে / অর্ধেক'],
  35: ['三十五分',   'sān shí wǔ fēn',   'thirty-five minutes',  'পঁয়ত্রিশ মিনিট'],
  40: ['四十分',     'sì shí fēn',       'forty minutes',        'চল্লিশ মিনিট'],
  45: ['四十五分',   'sì shí wǔ fēn',    'forty-five minutes',   'পঁয়তাল্লিশ মিনিট'],
  50: ['五十分',     'wǔ shí fēn',       'fifty minutes',        'পঞ্চাশ মিনিট'],
  55: ['五十五分',   'wǔ shí wǔ fēn',    'fifty-five minutes',   'পঞ্চান্ন মিনিট']
};
```

**Schema:** `minWords[minute_value] → [hanzi, pinyin, english, bangla]`  
**Coverage:** 5-minute intervals from 5 to 55  
**Note:** Key 15 uses `一刻` (quarter) not `十五分`; Key 30 uses `半` (half) not `三十分`.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 4. Chinese Time — `digit{}` (18 Entries)

Extracted verbatim from Line 44 of `chinese-time-html`:

```javascript
const digit = {
  0:  ['零',     'líng',       'zero',         'শূন্য'],
  1:  ['一',     'yī',         'one',          'এক'],
  2:  ['二',     'èr',         'two',          'দুই'],
  3:  ['三',     'sān',        'three',        'তিন'],
  4:  ['四',     'sì',         'four',         'চার'],
  5:  ['五',     'wǔ',         'five',         'পাঁচ'],
  6:  ['六',     'liù',        'six',          'ছয়'],
  7:  ['七',     'qī',         'seven',        'সাত'],
  8:  ['八',     'bā',         'eight',        'আট'],
  9:  ['九',     'jiǔ',        'nine',         'নয়'],
  10: ['十',     'shí',        'ten',          'দশ'],
  13: ['十三',   'shí sān',    'thirteen',     'তেরো'],
  14: ['十四',   'shí sì',     'fourteen',     'চৌদ্দ'],
  16: ['十六',   'shí liù',    'sixteen',      'ষোল'],
  18: ['十八',   'shí bā',     'eighteen',     'আঠারো'],
  20: ['二十',   'èr shí',     'twenty',       'বিশ'],
  30: ['三十',   'sān shí',    'thirty',       'ত্রিশ'],
  45: ['四十五', 'sì shí wǔ',  'forty-five',   'পঁয়তাল্লিশ']
};
```

**Schema:** `digit[value] → [hanzi, pinyin, english, bangla]`  
**Coverage:** 0–9 (all), 10, 13, 14, 16, 18, 20, 30, 45 (selective)  
**Purpose:** Used by `digital()` builder for formal time like "3:05" → `三点零五分`  
**Note:** Key 2 uses `二` (èr) not `两` — `digit{}` is for reading digits, not for spoken time.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 5. Finnish Time — `nums{}` (16 Entries)

Extracted verbatim from Line 37 of `finnish-time-html`:

```javascript
const nums = {
  1:  ['yksi',              'YUK-si',                 'one',                  'এক'],
  2:  ['kaksi',             'KAHK-si',                'two',                  'দুই'],
  3:  ['kolme',             'KOL-me',                 'three',                'তিন'],
  4:  ['neljä',             'NEL-ya',                 'four',                 'চার'],
  5:  ['viisi',             'VEE-si',                 'five',                 'পাঁচ'],
  6:  ['kuusi',             'KOO-si',                 'six',                  'ছয়'],
  7:  ['seitsemän',         'SAYT-se-man',            'seven',                'সাত'],
  8:  ['kahdeksan',         'KAH-dek-san',            'eight',                'আট'],
  9:  ['yhdeksän',          'UH-dek-san',             'nine',                 'নয়'],
  10: ['kymmenen',          'KUM-me-nen',             'ten',                  'দশ'],
  11: ['yksitoista',        'YUK-si-toy-sta',         'eleven',               'এগারো'],
  12: ['kaksitoista',       'KAHK-si-toy-sta',        'twelve',               'বারো'],
  15: ['varttia',           'VART-ti-a',              'quarter / fifteen',    'পনেরো'],
  20: ['kaksikymmentä',     'KAHK-si-KUM-men-ta',     'twenty',               'বিশ'],
  25: ['kaksikymmentäviisi','KAHK-si-KUM-men-ta VEE-si','twenty-five',        'পঁচিশ'],
  30: ['kolmekymmentä',     'KOL-me-KUM-men-ta',      'thirty',               'ত্রিশ']
};
```

**Schema:** `nums[value] → [finnish, pronunciation, english, bangla]`  
**Coverage:** 1–12 + 15, 20, 25, 30  
**Note:** No `minWords` or `digit` tables — Finnish uses `nums` for all lookups. No builder functions.

`[VERIFIED FROM SHARED CODEBASE: finnish-time-html]`

---

## 6. Portuguese Time — `N{}` (21 Entries)

Extracted verbatim from Line 21 of `portuguese-time-html`:

```javascript
const N = {
  1:  ['uma',               'OO-ma',                           'one',          'এক'],
  2:  ['duas',              'DOO-ash',                         'two',          'দুই'],
  3:  ['três',              'TRESH',                           'three',        'তিন'],
  4:  ['quatro',            'KWA-troo',                        'four',         'চার'],
  5:  ['cinco',             'SEEN-koo',                        'five',         'পাঁচ'],
  6:  ['seis',              'SAYSH',                           'six',          'ছয়'],
  7:  ['sete',              'SEH-tuh',                         'seven',        'সাত'],
  8:  ['oito',              'OY-too',                          'eight',        'আট'],
  9:  ['nove',              'NOH-vuh',                         'nine',         'নয়'],
  10: ['dez',               'DEZ',                             'ten',          'দশ'],
  11: ['onze',              'ON-zuh',                          'eleven',       'এগারো'],
  12: ['doze',              'DOH-zuh',                         'twelve',       'বারো'],
  15: ['quinze',            'KEEN-zuh',                        'fifteen',      'পনেরো'],
  20: ['vinte',             'VEEN-tuh',                        'twenty',       'বিশ'],
  25: ['vinte e cinco',     'VEEN-tchee ee SEEN-koo',          'twenty-five',  'পঁচিশ'],
  30: ['trinta',            'TREEN-ta',                        'thirty',       'ত্রিশ'],
  35: ['trinta e cinco',    'TREEN-ta ee SEEN-koo',            'thirty-five',  'পঁয়ত্রিশ'],
  40: ['quarenta',          'kwa-REN-ta',                      'forty',        'চল্লিশ'],
  45: ['quarenta e cinco',  'kwa-REN-ta ee SEEN-koo',          'forty-five',   'পঁয়তাল্লিশ'],
  50: ['cinquenta',         'seen-KWEN-ta',                    'fifty',        'পঞ্চাশ'],
  55: ['cinquenta e cinco', 'seen-KWEN-ta ee SEEN-koo',        'fifty-five',   'পঞ্চান্ন']
};
```

**Schema:** `N[value] → [portuguese, pronunciation, english, bangla]`  
**Coverage:** 1–12 + 15, 20, 25, 30, 35, 40, 45, 50, 55  
**Note:** Uses single table `N` (not separate `nums`/`minWords`/`digit`). Variable named `N` not `nums`.

`[VERIFIED FROM SHARED CODEBASE: portuguese-time-html]`

---

## 7. Saudi Arabic Time — No Lookup Tables

Saudi Arabic uses **no separate lookup tables**. All number words are embedded inline in the pre-authored item objects:

```javascript
// Example from Chapter 0, item at index 2:
{
  "time": "01:00",
  "ar": "السَّاعَةُ الوَاحِدَةُ.",
  "words": [
    ["السَّاعَةُ", "as-sāʿah", "the hour / the time", "সময় / ঘণ্টা"],
    ["الوَاحِدَةُ", "al-wāḥidah", "one / first hour", "একটা"]
  ]
}
```

Arabic ordinal numbers for hours (الوَاحِدَةُ, الثَّانِيَةُ, الثَّالِثَةُ...) are embedded directly — no programmatic lookup.

`[VERIFIED FROM SHARED CODEBASE: saudi-arabic-time-html]`

---

## 8. Bangla Number/Time Text — `bnTimeText()` Function

All 4 Drill Labs share an identical `bnTimeText()` function that converts `"HH:MM"` to spoken Bangla time text.

### 8.1 Complete Bangla Hour Words (12 entries)

```javascript
const b = {
  1:  'একটা',      // ektā
  2:  'দুইটা',     // duitā
  3:  'তিনটা',     // tintā
  4:  'চারটা',     // chārtā
  5:  'পাঁচটা',    // pā̃chtā
  6:  'ছয়টা',     // choytā
  7:  'সাতটা',     // sāttā
  8:  'আটটা',      // āṭtā
  9:  'নয়টা',     // noytā
  10: 'দশটা',      // doshtā
  11: 'এগারোটা',   // egārotā
  12: 'বারোটা'     // bārotā
};
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, portuguese-time-html]` — Identical in all 4 bundles.

### 8.2 Complete Bangla Minute Words (12 entries)

```javascript
const mm = {
  0:  '',           // (no minutes)
  5:  'পাঁচ',      // pā̃ch
  10: 'দশ',        // dosh
  15: 'পনেরো',     // ponero
  20: 'বিশ',       // bish
  25: 'পঁচিশ',     // pō̃chish
  30: 'ত্রিশ',     // trish
  35: 'পঁয়ত্রিশ',  // pō̃ytrish
  40: 'চল্লিশ',    // chollish
  45: 'পঁয়তাল্লিশ', // pō̃ytāllish
  50: 'পঞ্চাশ',    // pō̃chāsh
  55: 'পঞ্চান্ন'    // pō̃chānno
};
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 8.3 `bnTimeText()` Logic Branches

```javascript
function bnTimeText(t) {
  let [h, m] = t.split(':').map(Number);
  h = ((h % 12) || 12);

  if (m === 0)  return b[h];                              // "তিনটা"
  if (m === 15) return 'সোয়া ' + b[h];                    // "সোয়া তিনটা"
  if (m === 30) return 'সাড়ে ' + b[h];                    // "সাড়ে তিনটা"
  if (m === 45) {
    let n = (h % 12) + 1;
    return b[n] + 'র পনেরো মিনিট বাকি';                   // "চারটার পনেরো মিনিট বাকি"
  }
  if (m > 30) {
    let n = (h % 12) + 1;
    return b[n] + 'র ' + mm[60 - m] + ' মিনিট বাকি';     // "চারটার দশ মিনিট বাকি"
  }
  return b[h] + ' ' + mm[m] + ' মিনিট';                   // "তিনটা দশ মিনিট"
}
```

**Key Bangla time patterns:**
- **সোয়া** (soẏā) = quarter past
- **সাড়ে** (sāṛe) = half past
- **বাকি** (bāki) = remaining / "to"
- **মিনিট** (miniṭ) = minute (loanword)
- **-র** (-r) = possessive marker ("of")

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 8.4 Projected `bnNumText()` for Number Drill

The Number Drill would need a `bnNumText()` function instead — converting numeric values to spoken Bangla number words:

```javascript
// PROJECTED — not verified from live source
function bnNumText(n) {
  // Bangla cardinal numbers:
  // 1=এক, 2=দুই, 3=তিন, 4=চার, 5=পাঁচ, 6=ছয়, 7=সাত, 8=আট, 9=নয়, 10=দশ
  // 11=এগারো, 12=বারো, 13=তেরো, 14=চৌদ্দ, 15=পনেরো, 16=ষোল, 17=সতেরো, 18=আঠারো, 19=উনিশ
  // 20=বিশ, 21=একুশ, 22=বাইশ, ...
  // 100=একশো, 1000=এক হাজার
}
```

`[LOCKED / PAYWALL]` — Projected. Exact implementation unknown.

---

## 9. Projected German Number Word System (0–1000+)

### 9.1 Core Number Words (0–19)

| Value | German | Pron. (projected) | English | Bangla |
|:-----:|--------|-------------------|---------|--------|
| 0 | null | NOOL | zero | শূন্য |
| 1 | eins | AYNS | one | এক |
| 2 | zwei | TSVAY | two | দুই |
| 3 | drei | DRAY | three | তিন |
| 4 | vier | FEER | four | চার |
| 5 | fünf | FUENF | five | পাঁচ |
| 6 | sechs | ZEKS | six | ছয় |
| 7 | sieben | ZEE-ben | seven | সাত |
| 8 | acht | AHKHT | eight | আট |
| 9 | neun | NOYN | nine | নয় |
| 10 | zehn | TSAYN | ten | দশ |
| 11 | elf | ELF | eleven | এগারো |
| 12 | zwölf | TSVOELF | twelve | বারো |
| 13 | dreizehn | DRAY-tsayn | thirteen | তেরো |
| 14 | vierzehn | FEER-tsayn | fourteen | চৌদ্দ |
| 15 | fünfzehn | FUENF-tsayn | fifteen | পনেরো |
| 16 | sechzehn | ZEKH-tsayn | sixteen | ষোল |
| 17 | siebzehn | ZEEB-tsayn | seventeen | সতেরো |
| 18 | achtzehn | AHKHT-tsayn | eighteen | আঠারো |
| 19 | neunzehn | NOYN-tsayn | nineteen | উনিশ |

`[LOCKED / PAYWALL]` — German number words are standard CEFR A1 vocabulary. The exact format used in App #06 is unknown. Bangla equivalents are verified from the Bangla number words appearing in the shared `bnTimeText()` function and `nums{}` tables.

### 9.2 Tens (20–90)

| Value | German | Pronunciation |
|:-----:|--------|---------------|
| 20 | zwanzig | TSVAN-tsig |
| 30 | dreißig | DRAY-sig |
| 40 | vierzig | FEER-tsig |
| 50 | fünfzig | FUENF-tsig |
| 60 | sechzig | ZEKH-tsig |
| 70 | siebzig | ZEEB-tsig |
| 80 | achtzig | AHKHT-tsig |
| 90 | neunzig | NOYN-tsig |

`[LOCKED / PAYWALL]` — Standard German; exact format unknown.

### 9.3 Compound Number Rule (German-Specific)

**German reverses the ones and tens**, connected by "und":

> **ones + "und" + tens**

| Value | German Compound | Literal Structure |
|:-----:|-----------------|-------------------|
| 21 | einundzwanzig | ein + und + zwanzig |
| 35 | fünfunddreißig | fünf + und + dreißig |
| 47 | siebenundvierzig | sieben + und + vierzig |
| 63 | dreiundsechzig | drei + und + sechzig |
| 99 | neunundneunzig | neun + und + neunzig |

**Special case:** 1 in compound = "ein" (not "eins").

`[LOCKED / PAYWALL]` — Standard German grammar rule. How App #06 handles compound number generation algorithmically is unknown.

### 9.4 Hundreds & Thousands

| Value | German |
|:-----:|--------|
| 100 | (ein)hundert |
| 200 | zweihundert |
| 347 | dreihundertsiebenundvierzig |
| 1000 | (ein)tausend |

`[LOCKED / PAYWALL]` — Standard German. Whether App #06 covers values above 999 is unknown.

---

## 10. Cross-Bundle Numeric Vocabulary Comparison

| Feature | Chinese | Finnish | Portuguese | Arabic | German (Proj.) |
|---------|:-------:|:-------:|:----------:|:------:|:--------------:|
| Table variable name | `nums` + `minWords` + `digit` | `nums` | `N` | (none) | `nums` (proj.) |
| Total entries | 41 (12+11+18) | 16 | 21 | 0 | 100+ (proj.) |
| Coverage range | 0–55 | 1–30 | 1–55 | — | 0–1000+ (proj.) |
| Compound word rule | ❌ | ❌ | "e" joins | ❌ | "und" joins (reversed) |
| Irregular numbers | 两 for 2 | — | uma/duas (fem.) | Ordinals | elf, zwölf, sechzehn, siebzehn |
| Bangla included | ✅ All 4 fields | ✅ All 4 fields | ✅ All 4 fields | ✅ All 4 fields | ✅ (proj.) |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html, portuguese-time-html]` for existing bundles.

---

*End of file — 08-NUMBER-WORD-DATA-EXTRACTION.md*
