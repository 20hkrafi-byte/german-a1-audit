# 09 — Complete Time Data Extraction: German A1 App #07 "Time, Date & Schedule Drill"

**Audit Date:** 2026-11-24  
**Source Bundles:** All 4 Time Drill Labs — verbatim free-chapter data  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. Chinese Time — Chapter 1 (5 Items, Verbatim)

```javascript
const chapters = [{
  title: 'Chapter 1 — Chinese Clock System',
  items: [
    exact(3),              // Item 1
    past(3, 10),           // Item 2
    quarterPast(3),        // Item 3
    half(3),               // Item 4
    toTime(3, 50, 4, 10)   // Item 5
  ]
}];
```

### Expanded Items

**Item 1 — exact(3):**

| Field | Value |
|-------|-------|
| time | `03:00` |
| de | `现在三点。` |
| pron | `Xiànzài sān diǎn.` |
| en | `It is 3 o'clock.` |
| bn | `এখন তিনটা।` |
| words | `[['现在','xiànzài','now','এখন'], ['三','sān','three','তিন'], ['点','diǎn',"o'clock / hour marker",'টা / ঘণ্টা-চিহ্ন']]` |

**Item 2 — past(3,10):**

| Field | Value |
|-------|-------|
| time | `03:10` |
| de | `现在三点十分。` |
| pron | `Xiànzài sān diǎn shí fēn.` |
| en | `It is 10 past 3.` |
| bn | `এখন তিনটা দশ মিনিট।` |
| words | `[['现在','xiànzài','now','এখন'], ['三','sān','three','তিন'], ['点','diǎn',"o'clock / hour marker",'টা / ঘণ্টা-চিহ্ন'], ['十分','shí fēn','ten minutes','দশ মিনিট']]` |

**Item 3 — quarterPast(3):**

| Field | Value |
|-------|-------|
| time | `03:15` |
| de | `现在三点一刻。` |
| pron | `Xiànzài sān diǎn yí kè.` |
| en | `It is quarter past 3.` |
| bn | `এখন সোয়া তিনটা।` |
| words | `[['现在','xiànzài','now','এখন'], ['三','sān','three','তিন'], ['点','diǎn',"o'clock / hour marker",'টা / ঘণ্টা-চিহ্ন'], ['一刻','yí kè','quarter','সোয়া / পনেরো মিনিট']]` |

**Item 4 — half(3):**

| Field | Value |
|-------|-------|
| time | `03:30` |
| de | `现在三点半。` |
| pron | `Xiànzài sān diǎn bàn.` |
| en | `It is half past 3.` |
| bn | `এখন সাড়ে তিনটা।` |
| words | `[['现在','xiànzài','now','এখন'], ['三','sān','three','তিন'], ['点','diǎn',"o'clock / hour marker",'টা / ঘণ্টা-চিহ্ন'], ['半','bàn','half','সাড়ে / অর্ধেক']]` |

**Item 5 — toTime(3,50,4,10):**

| Field | Value |
|-------|-------|
| time | `03:50` |
| de | `现在差十分四点。` |
| pron | `Xiànzài chà shí fēn sì diǎn.` |
| en | `It is 10 to 4.` |
| bn | `এখন চারটার দশ মিনিট বাকি।` |
| words | `[['现在','xiànzài','now','এখন'], ['差','chà','short of / to','বাকি'], ['十分','shí fēn','ten minutes','দশ মিনিট'], ['四','sì','four','চার'], ['点','diǎn',"o'clock",'টা']]` |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 55–62.

---

## 2. Finnish Time — Chapter 0 (5 Items, Verbatim)

**Item 1:**

| Field | Value |
|-------|-------|
| time | `3:00` |
| nl | `Kello on kolme.` |
| pron | `KEL-lo on KOL-me` |
| en | `It is three o'clock.` |
| bn | `এখন তিনটা।` |
| words | `[['Kello','KEL-lo','clock / time','ঘড়ি / সময়'], ['on','on','is','হয় / আছে'], ['kolme','KOL-me','three','তিন']]` |

**Item 2:**

| Field | Value |
|-------|-------|
| time | `3:10` |
| nl | `Kello on kymmenen yli kolme.` |
| pron | `KEL-lo on KUM-me-nen U-li KOL-me` |
| en | `It is ten past three.` |
| bn | `এখন তিনটা দশ।` |
| words | `[['kymmenen','KUM-me-nen','ten','দশ'], ['yli','U-li','past / over','বেশি / পেরিয়ে'], ['kolme','KOL-me','three','তিন']]` |

**Item 3:**

| Field | Value |
|-------|-------|
| time | `3:15` |
| nl | `Kello on varttia yli kolme.` |
| pron | `KEL-lo on VART-ti-a U-li KOL-me` |
| en | `It is quarter past three.` |
| bn | `এখন সোয়া তিনটা।` |
| words | `[['varttia','VART-ti-a','quarter','পনেরো মিনিট'], ['yli','U-li','past','বেশি'], ['kolme','KOL-me','three','তিন']]` |

**Item 4:**

| Field | Value |
|-------|-------|
| time | `3:30` |
| nl | `Kello on puoli neljä.` |
| pron | `KEL-lo on PUO-li NEL-ya` |
| en | `It is half past three. Finnish says "half four".` |
| bn | `এখন সাড়ে তিনটা। ফিনিশে: চারটার অর্ধেক আগে।` |
| words | `[['puoli','PUO-li','half before next hour','পরের ঘণ্টার অর্ধেক আগে'], ['neljä','NEL-ya','four','চার']]` |

**Item 5:**

| Field | Value |
|-------|-------|
| time | `3:45` |
| nl | `Kello on varttia vaille neljä.` |
| pron | `KEL-lo on VART-ti-a VAI-le NEL-ya` |
| en | `It is quarter to four.` |
| bn | `এখন চারটার পনেরো মিনিট বাকি।` |
| words | `[['varttia','VART-ti-a','quarter','পনেরো মিনিট'], ['vaille','VAI-le','to / before','আগে / বাকি'], ['neljä','NEL-ya','four','চার']]` |

`[VERIFIED FROM SHARED CODEBASE: finnish-time-html]` — Line 46.

---

## 3. Portuguese Time — Chapter 1 (7 Items, Verbatim)

**Item 1 — exact(1):**

| Field | Value |
|-------|-------|
| time | `1:00` |
| de | `É uma hora.` |
| pron | `EH OO-ma OH-ra` |
| en | `It is 1 o'clock.` |
| bn | `এখন একটা।` |
| words | `[['É','EH','it is','এখন / হয়'], ['uma','OO-ma','one','এক'], ['hora','OH-ra(s)','hour(s) / o\'clock','টা / ঘণ্টা']]` |

**Item 2 — exact(3):**

| Field | Value |
|-------|-------|
| time | `3:00` |
| de | `São três horas.` |
| pron | `SOWN TRESH OH-rash` |
| en | `It is 3 o'clock.` |
| bn | `এখন তিনটা।` |
| words | `[['São','SOWN','it is','এখন / হয়'], ['três','TRESH','three','তিন'], ['horas','OH-ra(s)','hour(s) / o\'clock','টা / ঘণ্টা']]` |

**Item 3 — quart(3):**

| Field | Value |
|-------|-------|
| time | `3:15` |
| de | `São três e um quarto.` |
| pron | `SOWN TRESH ee oom KWAR-too` |
| en | `It is quarter past 3.` |
| bn | `এখন সোয়া তিনটা।` |
| words | `[['São','SOWN','it is','এখন / হয়'], ['três','TRESH','three','তিন'], ['e','ee','and','এবং'], ['um quarto','oom KWAR-too','a quarter','পনেরো মিনিট']]` |

**Item 4 — half(3):**

| Field | Value |
|-------|-------|
| time | `3:30` |
| de | `São três e meia.` |
| pron | `SOWN TRESH ee MAY-a` |
| en | `It is half past 3.` |
| bn | `এখন সাড়ে তিনটা।` |
| words | `[['São','SOWN','it is','এখন / হয়'], ['três','TRESH','three','তিন'], ['e','ee','and','এবং'], ['meia','MAY-a','half','আধা / সাড়ে']]` |

**Item 5 — para(3,45,4,15):**

| Field | Value |
|-------|-------|
| time | `3:45` |
| de | `São quinze para as quatro.` |
| pron | `SOWN KEEN-zuh PA-ra ash KWA-troo` |
| en | `It is 15 to 4.` |
| bn | `এখন চারটার পনেরো মিনিট বাকি।` |
| words | `[['São','SOWN','it is / they are','এখন / হয়'], ['quinze','KEEN-zuh','fifteen','পনেরো'], ['para','PA-ra','to / before','বাকি / আগে'], ['as','as','the','নির্দিষ্ট article'], ['quatro','KWA-troo','four','চার']]` |

**Item 6 — noon:**

| Field | Value |
|-------|-------|
| time | `12:00` |
| de | `É meio-dia.` |
| pron | `EH MAY-oo DEE-a` |
| en | `It is noon.` |
| bn | `এখন দুপুর বারোটা।` |
| words | `[['É','EH','it is','এখন / হয়'], ['meio-dia','MAY-oo DEE-a','noon','দুপুর বারোটা']]` |

**Item 7 — midnight:**

| Field | Value |
|-------|-------|
| time | `00:00` |
| de | `É meia-noite.` |
| pron | `EH MAY-a NOY-tchee` |
| en | `It is midnight.` |
| bn | `এখন রাত বারোটা।` |
| words | `[['É','EH','it is','এখন / হয়'], ['meia-noite','MAY-a NOY-tchee','midnight','রাত বারোটা']]` |

`[VERIFIED FROM SHARED CODEBASE: portuguese-time-html]` — Lines 23–29.

---

## 4. Saudi Arabic Time — Chapter 0 (9 Items, Verbatim)

**Item 1 — "What time?" (formal):**

| Field | Value |
|-------|-------|
| time | `00:00` |
| ar | `كَمِ السَّاعَةُ؟` |
| pron | `kami as-sāʿah?` |
| en | `What time is it?` |
| bn | `কয়টা বাজে?` |
| words | `[['كَمِ','kami','how much / what','কত'], ['السَّاعَةُ','as-sāʿah','the time / the clock','সময় / ঘড়ি']]` |

**Item 2 — "What time?" (alternative):**

| Field | Value |
|-------|-------|
| time | `00:00` |
| ar | `مَا الوَقْتُ الآنَ؟` |
| pron | `mā al-waqtu al-ʾān?` |
| en | `What is the time now?` |
| bn | `এখন সময় কত?` |
| words | `[['مَا','mā','what','কি'], ['الوَقْتُ','al-waqt','the time','সময়'], ['الآنَ','al-ʾān','now','এখন']]` |

**Item 3 — 1:00:**

| Field | Value |
|-------|-------|
| ar | `السَّاعَةُ الوَاحِدَةُ.` |
| pron | `as-sāʿatu al-wāḥidah` |
| en | `It is 1 o'clock.` |
| bn | `এখন একটা।` |
| words | `[['السَّاعَةُ','as-sāʿah','the hour / the time','সময় / ঘণ্টা'], ['الوَاحِدَةُ','al-wāḥidah','one / first hour','একটা']]` |

**Item 4 — 2:00:**

| Field | Value |
|-------|-------|
| ar | `السَّاعَةُ الثَّانِيَةُ.` |
| pron | `as-sāʿatu ath-thāniyah` |
| en | `It is 2 o'clock.` |
| bn | `এখন দুইটা।` |

**Item 5 — 3:15:**

| Field | Value |
|-------|-------|
| ar | `السَّاعَةُ الثَّالِثَةُ وَالرُّبْعُ.` |
| pron | `as-sāʿatu ath-thālithah war-rubʿ` |
| en | `It is quarter past 3.` |
| bn | `এখন সোয়া তিনটা।` |
| words | `[['السَّاعَةُ','as-sāʿah',...], ['الثَّالِثَةُ','ath-thālithah','three / third hour','তিনটা'], ['وَ','wa','and','এবং'], ['الرُّبْعُ','ar-rubʿ','the quarter','সোয়া / পনেরো মিনিট']]` |

**Item 6 — 4:30:**

| Field | Value |
|-------|-------|
| ar | `السَّاعَةُ الرَّابِعَةُ وَالنِّصْفُ.` |
| pron | `as-sāʿatu ar-rābiʿah wan-niṣf` |
| en | `It is half past 4.` |
| bn | `এখন সাড়ে চারটা।` |

**Item 7 — 5:45:**

| Field | Value |
|-------|-------|
| ar | `السَّاعَةُ السَّادِسَةُ إِلَّا الرُّبْعَ.` |
| pron | `as-sāʿatu as-sādisah illā ar-rubʿ` |
| en | `It is quarter to 6.` |
| bn | `এখন ছয়টার পনেরো মিনিট বাকি।` |

**Item 8 — 6:10:**

| Field | Value |
|-------|-------|
| ar | `السَّاعَةُ السَّادِسَةُ وَعَشْرُ دَقَائِقَ.` |
| pron | `as-sāʿatu as-sādisah wa-ʿashru daqāʾiq` |
| en | `It is 10 minutes past 6.` |
| bn | `এখন ছয়টা দশ মিনিট।` |

**Item 9 — 7:50:**

| Field | Value |
|-------|-------|
| ar | `السَّاعَةُ الثَّامِنَةُ إِلَّا عَشْرُ دَقَائِقَ.` |
| pron | `as-sāʿatu ath-thāminah illā ʿashru daqāʾiq` |
| en | `It is 10 minutes to 8.` |
| bn | `এখন আটটার দশ মিনিট বাকি।` |

`[VERIFIED FROM SHARED CODEBASE: saudi-arabic-time-html]` — Line 57.

---

## 5. Cross-Bundle Time Pattern Coverage

| Time Pattern | Chinese | Finnish | Portuguese | Arabic |
|-------------|:-------:|:-------:|:----------:|:------:|
| Exact hour | ✅ 3:00 | ✅ 3:00 | ✅ 1:00, 3:00 | ✅ 1:00, 2:00 |
| Minutes past | ✅ 3:10 | ✅ 3:10 | — | ✅ 6:10 |
| Quarter past | ✅ 3:15 | ✅ 3:15 | ✅ 3:15 | ✅ 3:15 |
| Half past | ✅ 3:30 | ✅ 3:30 | ✅ 3:30 | ✅ 4:30 |
| Quarter to | — | ✅ 3:45 | ✅ 3:45 | ✅ 5:45 |
| Minutes to | ✅ 3:50 | — | — | ✅ 7:50 |
| Noon | — | — | ✅ 12:00 | — |
| Midnight | — | — | ✅ 00:00 | — |
| "What time?" | — | — | — | ✅ (×2) |
| **Total items** | **5** | **5** | **7** | **9** |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

*End of file — 09-COMPLETE-TIME-DATA-EXTRACTION.md*
