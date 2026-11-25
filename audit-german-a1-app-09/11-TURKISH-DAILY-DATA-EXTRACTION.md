# 11 — Turkish Daily Command Data Extraction: Chapter 1 (All 6 Scenarios)

> **App:** German A1 — #09 "Daily Conversation Drill"
> **Source Bundle:** `turkish-daily-html` (line 86)
> **Verification:** `[VERIFIED FROM SHARED CODEBASE: turkish-daily-html]`

---

## 1. Chapter Catalogue (All 10 Chapters)

| # | Turkish Title | English Title | Bangla Title | Icon | Status |
|:-:|---------------|---------------|--------------|:----:|:------:|
| 1 | Mutfak ve Yemekler | Kitchen & Meals | রান্নাঘর ও খাবার | 🍽️ | ✅ Open |
| 2 | Sabah ve Yatak Odası | Morning & Bedroom | সকাল ও শোবার ঘর | 🛏️ | 🔒 Locked |
| 3 | Banyo ve Kişisel Bakım | Bathroom & Personal Care | বাথরুম ও ব্যক্তিগত পরিচর্যা | 🪥 | 🔒 Locked |
| 4 | Temizlik ve Çamaşır | Cleaning & Laundry | পরিষ্কার-পরিচ্ছন্নতা ও কাপড় ধোয়া | 🧹 | 🔒 Locked |
| 5 | Evden Çıkma | Leaving Home | বাড়ি থেকে বের হওয়া | 🚪 | 🔒 Locked |
| 6 | Alışveriş ve Günlük İşler | Shopping & Errands | কেনাকাটা ও দৈনন্দিন কাজ | 🛒 | 🔒 Locked |
| 7 | Sokak ve Ulaşım | Street & Transport | রাস্তা ও যাতায়াত | 🚌 | 🔒 Locked |
| 8 | İş ve Ders | Work & Study | কাজ ও পড়াশোনা | 💻 | 🔒 Locked |
| 9 | Salon ve Aile | Living Room & Family | বসার ঘর ও পরিবার | 🏠 | 🔒 Locked |
| 10 | Güvenlik ve Küçük Acil Durumlar | Safety & Small Emergencies | নিরাপত্তা ও ছোট জরুরি পরিস্থিতি | 🧯 | 🔒 Locked |

---

## 2. Grammar Rule (Chapter 1)

**English:**
> Turkish command order: the verb normally comes last. A destination takes the **dative -(y)e/-(y)a**: *mutfağa git, masaya getir*. A specific object commonly takes the **accusative -(y)i/ı/u/ü**: *dolabı aç, şekeri getir*. The everyday singular command is usually the bare verb stem: *git, aç, getir*.

**Bangla:**
> তুর্কি command-এ verb সাধারণত শেষে থাকে। গন্তব্যে **-(y)e/-(y)a** dative লাগে: *mutfağa git, masaya getir*। নির্দিষ্ট object-এ **-(y)i/ı/u/ü** accusative লাগে: *dolabı aç, şekeri getir*। দৈনন্দিন singular command সাধারণত verb stem: *git, aç, getir*।

`[VERIFIED FROM SHARED CODEBASE: turkish-daily-html]`

---

## 3. Scenario 1: Şekeri getir (Bring the sugar)

**Goal:** Amaç: Şekeri getir.
**EN:** Bring the sugar | **BN:** চিনি এনে দাও
**Scene:** kitchen

### Props
| ID | Emoji | Label | X | Y |
|----|:-----:|-------|:-:|:-:|
| cupboard | 🗄️ | Dolap | 72 | 29 |
| sugar | 🧂 | Şeker | 72 | 43 |
| table | 🍽️ | Masa | 25 | 67 |

### Steps (3)
| # | Turkish | English | Bangla | Action | Target | W4W |
|:-:|---------|---------|--------|:------:|--------|-----|
| 1 | Mutfağa git. | Go to the kitchen. | রান্নাঘরে যাও। | move | — | Mutfağa = to the kitchen / রান্নাঘরে · git = go / যাও |
| 2 | Dolabı aç. | Open the cupboard. | আলমারিটি খোলো। | open | cupboard | Dolabı = the cupboard / আলমারিটি · aç = open / খোলো |
| 3 | Şekeri masaya getir. | Bring the sugar to the table. | চিনিটা টেবিলে নিয়ে আসো। | bring | sugar | Şekeri = the sugar / চিনিটা · masaya = to the table / টেবিলে · getir = bring / নিয়ে আসো |

---

## 4. Scenario 2: Bir fincan çay hazırla (Make a cup of tea)

**Goal:** Amaç: Bir fincan çay hazırla.
**EN:** Make a cup of tea | **BN:** এক কাপ চা বানাও
**Scene:** kitchen

### Props
| ID | Emoji | Label | X | Y |
|----|:-----:|-------|:-:|:-:|
| kettle | 🫖 | Su ısıtıcısı | 69 | 55 |
| cup | ☕ | Fincan | 31 | 62 |
| tea | 🫙 | Çay | 73 | 33 |
| sink | 🚰 | Musluk | 47 | 47 |

### Steps (4)
| # | Turkish | English | Bangla | Action | Target | W4W |
|:-:|---------|---------|--------|:------:|--------|-----|
| 1 | Su ısıtıcısını suyla doldur. | Fill the kettle with water. | কেটলিতে পানি ভরো। | use | sink | Su = water / পানি · ısıtıcısını = the kettle / কেটলিটি · suyla = with water / পানি দিয়ে · doldur = fill / ভরো |
| 2 | Su ısıtıcısını çalıştır. | Switch on the kettle. | কেটলি চালু করো। | on | kettle | Su = water / পানি · ısıtıcısını = the kettle / কেটলিটি · çalıştır = switch on / start / চালু করো |
| 3 | Çayı fincana koy. | Put the tea in the cup. | কাপে চা দাও। | put | tea | Çayı = the tea / চা · fincana = into the cup / কাপে · koy = put / রাখো |
| 4 | Sıcak suyu fincana dök. | Pour the hot water into the cup. | কাপে গরম পানি ঢালো। | pour | kettle | Sıcak = hot / গরম · suyu = the water / পানি · fincana = into the cup / কাপে · dök = pour / ঢালো |

---

## 5. Scenario 3: Kahvaltıyı hazırla (Prepare breakfast)

**Goal:** Amaç: Kahvaltıyı hazırla.
**EN:** Prepare breakfast | **BN:** সকালের নাশতা প্রস্তুত করো
**Scene:** kitchen

### Props
| ID | Emoji | Label | X | Y |
|----|:-----:|-------|:-:|:-:|
| fridge | 🧊 | Buzdolabı | 78 | 37 |
| bread | 🍞 | Ekmek | 31 | 47 |
| cheese | 🧀 | Peynir | 78 | 53 |
| plate | 🍽️ | Tabak | 29 | 65 |

### Steps (4)
| # | Turkish | English | Bangla | Action | Target | W4W |
|:-:|---------|---------|--------|:------:|--------|-----|
| 1 | Buzdolabını aç. | Open the fridge. | ফ্রিজ খোলো। | open | fridge | Buzdolabını = the fridge / ফ্রিজটি · aç = open / খোলো |
| 2 | Peyniri çıkar. | Take out the cheese. | চিজ বের করো। | take | cheese | Peyniri = the cheese / চিজটি · çıkar = take out / remove / বের করো |
| 3 | Ekmeği ve peyniri tabağa koy. | Put the bread and cheese on the plate. | পাউরুটি আর চিজ প্লেটে রাখো। | put | cheese | Ekmeği = the bread / রুটিটি · ve = and / এবং / আর · peyniri = the cheese / চিজটি · tabağa = onto the plate / প্লেটে · koy = put / রাখো |
| 4 | Tabağı masaya götür. | Take the plate to the table. | প্লেটটি টেবিলে নিয়ে যাও। | bring | plate | Tabağı = the plate / প্লেটটি · masaya = to the table / টেবিলে · götür = take / carry / নিয়ে যাও |

> [!NOTE]
> Step 3 has an `extraActions` field: `[{ action: "put", target: "bread", x: 29, y: 65 }]` — this moves both cheese AND bread to the plate simultaneously in the animated scene.

---

## 6. Scenario 4: Soğuk su getir (Bring cold water)

**Goal:** Amaç: Soğuk su getir.
**EN:** Bring cold water | **BN:** ঠান্ডা পানি নিয়ে আসো
**Scene:** kitchen

### Props
| ID | Emoji | Label | X | Y |
|----|:-----:|-------|:-:|:-:|
| fridge | 🧊 | Buzdolabı | 79 | 37 |
| water | 🧴 | Su | 78 | 53 |
| glass | 🥛 | Bardak | 28 | 62 |

### Steps (3)
| # | Turkish | English | Bangla | Action | Target | W4W |
|:-:|---------|---------|--------|:------:|--------|-----|
| 1 | Buzdolabını aç. | Open the fridge. | ফ্রিজ খোলো। | open | fridge | Buzdolabını = the fridge / ফ্রিজটি · aç = open / খোলো |
| 2 | Su şişesini çıkar. | Take out the bottle of water. | পানির বোতল বের করো। | take | water | Su = water / পানি · şişesini = the bottle / বোতলটি · çıkar = take out / remove / বের করো |
| 3 | Suyu getir ve bardağı doldur. | Bring the water and fill the glass. | পানি নিয়ে এসে গ্লাস ভরো। | bring | water | Suyu = the water / পানি · getir = bring / নিয়ে আসো · ve = and / এবং / আর · bardağı = the glass / গ্লাসটি · doldur = fill / ভরো |

> [!NOTE]
> Step 3 has `extraActions`: `[{ action: "pour", target: "water", x: 28, y: 62 }]` — pours water into glass after bringing it.

---

## 7. Scenario 5: Artan yemeği kaldır (Put away the leftovers)

**Goal:** Amaç: Artan yemeği kaldır.
**EN:** Put away the leftovers | **BN:** বাকি খাবার গুছিয়ে রাখো
**Scene:** kitchen

### Props
| ID | Emoji | Label | X | Y |
|----|:-----:|-------|:-:|:-:|
| food | 🥘 | Yemek | 28 | 60 |
| box | 🥡 | Kap | 46 | 60 |
| fridge | 🧊 | Buzdolabı | 79 | 38 |

### Steps (4)
| # | Turkish | English | Bangla | Action | Target | W4W |
|:-:|---------|---------|--------|:------:|--------|-----|
| 1 | Yemeği bir kaba koy. | Put the food in a container. | খাবারটা একটি বাক্সে রাখো। | put | food | Yemeği = the food / meal / খাবারটি · bir = a / one / একটি / এক · kaba = into a container / একটি পাত্রে · koy = put / রাখো |
| 2 | Kabın kapağını iyice kapat. | Close the container properly. | বাক্সটি ভালো করে বন্ধ করো। | close | box | Kabın = of the container / পাত্রটির · kapağını = its lid / এর ঢাকনাটি · iyice = thoroughly / well / ভালোভাবে · kapat = close / turn off / বন্ধ করো |
| 3 | Buzdolabını aç. | Open the fridge. | ফ্রিজ খোলো। | open | fridge | Buzdolabını = the fridge / ফ্রিজটি · aç = open / খোলো |
| 4 | Kabı buzdolabına koy. | Put the container in the fridge. | বাক্সটি ফ্রিজে রাখো। | put | box | Kabı = the container / পাত্রটি · buzdolabına = into the fridge / ফ্রিজে · koy = put / রাখো |

---

## 8. Scenario 6: Bulaşıkları yıka (Wash the dishes)

**Goal:** Amaç: Bulaşıkları yıka.
**EN:** Wash the dishes | **BN:** বাসন ধোও
**Scene:** kitchen

### Props
| ID | Emoji | Label | X | Y |
|----|:-----:|-------|:-:|:-:|
| dishes | 🍽️ | Bulaşıklar | 31 | 59 |
| sink | 🚰 | Mutfak lavabosu | 52 | 49 |
| rack | 🧺 | Bulaşıklık | 72 | 59 |

### Steps (4)
| # | Turkish | English | Bangla | Action | Target | W4W |
|:-:|---------|---------|--------|:------:|--------|-----|
| 1 | Bulaşıkları lavaboya götür. | Take the dishes to the sink. | বাসনগুলো সিঙ্কে নিয়ে যাও। | bring | dishes | Bulaşıkları = the dishes / থালা-বাসনগুলো · lavaboya = to the sink / সিঙ্কে · götür = take / carry / নিয়ে যাও |
| 2 | Musluğu aç ve bulaşıkları yıka. | Turn on the tap and wash the dishes. | কল খুলে বাসন ধোও। | on | sink | Musluğu = the tap / faucet / কলটি · aç = turn on / চালু করো · ve = and / এবং / আর · bulaşıkları = the dishes / থালা-বাসনগুলো · yıka = wash / ধোও |
| 3 | Hepsini iyice durula. | Rinse everything well. | সবকিছু ভালো করে ধুয়ে নাও। | wash | dishes | Hepsini = all of them / সবগুলো · iyice = thoroughly / well / ভালোভাবে · durula = rinse / ধুয়ে নাও / কুলিয়ে নাও |
| 4 | Bulaşıkları bulaşıklığa koy. | Put the dishes on the drying rack. | বাসনগুলো ড্রাইং র‍্যাকে রাখো। | put | dishes | Bulaşıkları = the dishes / থালা-বাসনগুলো · bulaşıklığa = onto the dish rack / বাসন শুকানোর র‍্যাকে · koy = put / রাখো |

> [!NOTE]
> Step 2 has `extraActions`: `[{ action: "wash", target: "dishes" }]` — applies the wash state to dishes prop simultaneously.

---

## 9. Chapter 1 Statistics

| Metric | Value |
|--------|-------|
| **Total scenarios** | 6 |
| **Total steps** | 22 |
| **Total W4W words** | 69 |
| **Unique action types** | 7 (move, open, bring, use, on, put, pour, take, close, wash) |
| **Unique props** | 15 (cupboard, sugar, table, kettle, cup, tea, sink, fridge, bread, cheese, plate, water, glass, food, box, dishes, rack) |
| **Steps with extraActions** | 3 (Scenarios 3, 4, 6) |
| **Average steps per scenario** | 3.67 |
| **Average W4W words per step** | 3.14 |

---

## 10. Action Type Catalogue

| Action | Count | Description |
|--------|:-----:|-------------|
| `put` | 7 | Place object at destination |
| `open` | 5 | Open a container/appliance |
| `bring` | 4 | Carry object to location (actor moves) |
| `take` | 2 | Remove from container (stays in place) |
| `on` | 2 | Turn on / activate |
| `use` | 1 | Generic interaction (fill kettle) |
| `pour` | 1 | Pour liquid from one container to another |
| `close` | 1 | Close / seal container |
| `wash` | 1 | Wash / clean (via extraActions: 2 total) |
| `move` | 1 | Actor walks to position (no prop interaction) |

`[VERIFIED FROM SHARED CODEBASE: turkish-daily-html]`

---

*End of file — 11-TURKISH-DAILY-DATA-EXTRACTION.md*
