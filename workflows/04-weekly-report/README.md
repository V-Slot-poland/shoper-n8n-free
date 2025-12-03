# 📊 Tygodniowy Raport Sprzedaży + AI

Automatyczny workflow generujący kompleksowe raporty tygodniowe z analizą AI i porównaniem tygodniowym.

## ✨ Funkcje

- 🕐 **Automatyczne uruchamianie** każdy poniedziałek o 08:00
- 📊 **Pobieranie zamówień** z bieżącego tygodnia (poniedziałek-niedziela)
- 📈 **Porównanie tydzień do tygodnia** (WoW - Week over Week)
- 🤖 **Zaawansowana analiza AI** (GPT-4o-mini) w języku polskim
- 📆 **Dzienny breakdown** - szczegóły dla każdego dnia tygodnia
- 💬 **Wysyłka raportu** na Slack/Telegram
- 💰 **Wielowalutowość** (PLN, EUR, USD)
- ⚡ **Timezone-safe** (działa poprawnie na CET/UTC)

## 📋 Wymagania

- **n8n** (self-hosted lub cloud) - [pobierz](https://n8n.io)
- **Shoper** z dostępem do WebAPI
- **OpenAI API Key** (~$0.05-0.10/tydzień)
- **Slack** lub Telegram (opcjonalnie)

## 🚀 Instalacja

### Krok 1: Import workflow

1. Pobierz plik [workflow.json](workflow.json)
2. W n8n kliknij **"Import from File"**
3. Wybierz pobrany plik

### Krok 2: Konfiguracja Shoper API

**Utwórz użytkownika API:**

1. Panel Shoper → **Konfiguracja → Użytkownicy**
2. **Dodaj użytkownika** z uprawnieniami WebAPI
3. Zapisz login i hasło

**Wpisz credentials w workflow:**

W node'zie **"getTygodnioweZamowienia Tool"** zamień:

```javascript
const shopUrl = 'YOUR_SHOP.shoper.pl';      // np. sklep.shoper.pl
const username = 'YOUR_SHOPER_USERNAME';    // Login użytkownika API
const password = 'YOUR_SHOPER_PASSWORD';    // Hasło użytkownika
```

### Krok 3: Konfiguracja OpenAI

1. n8n → **Credentials → Add Credential → OpenAI**
2. Wklej API Key z [platform.openai.com](https://platform.openai.com/api-keys)
3. Wybierz to credential w node'zie **"OpenAI Chat Model"**

### Krok 4: Konfiguracja Slack

1. n8n → **Credentials → Add Credential → Slack**
2. Autoryzuj aplikację
3. Wybierz kanał w node'zie **"Send to Slack"**

### Krok 5: Test

1. Kliknij **"Execute workflow"**
2. Sprawdź raport na Slack
3. Gotowe! 🎉

## 📖 Jak to działa?

### Architektura

```
Schedule Trigger (Monday 08:00)
    ↓
AI Agent (GPT-4o-mini)
    ↓
getTygodnioweZamowienia Tool
    ├─ Calculate week ranges (Mon-Sun)
    ├─ POST /auth (Basic Auth)
    ├─ GET /orders (Bearer token, last 200)
    ├─ Filter by current week
    ├─ Filter by previous week
    └─ Calculate stats & WoW comparison
    ↓
AI Analysis (Polish)
    ├─ Week assessment
    ├─ Key metrics
    ├─ Trends analysis
    └─ Recommendations
    ↓
Send to Slack
```

### Szczegóły techniczne

**Logika tygodnia:**
- Tydzień = poniedziałek 00:00 → niedziela 23:59
- Bieżący tydzień obliczany od dzisiejszej daty
- Poprzedni tydzień = 7 dni wstecz od poniedziałku

**Autoryzacja Shoper:**
- Dwuetapowa: `POST /auth` → Bearer token
- Token ważny 30 dni
- [Dokumentacja API](https://developers.shoper.pl)

**Filtrowanie zamówień:**
- Pobieramy ostatnie 200 zamówień (pokrywa ~2 tygodnie)
- Filtrujemy client-side po dacie
- Shoper `filters[date]` nie działa poprawnie

**Metryki:**
- Liczba zamówień (bieżący vs poprzedni tydzień)
- Całkowity przychód
- Średnia wartość zamówienia
- Zmiana % (WoW - Week over Week)
- Dzienny breakdown z nazwami dni tygodnia

## 📸 Przykładowy raport

```
📊 TYGODNIOWY RAPORT SPRZEDAŻY

📅 OKRES
Bieżący tydzień: 2025-11-24 - 2025-11-30 (Pon-Niedz)
Poprzedni tydzień: 2025-11-17 - 2025-11-23

📊 BIEŻĄCY TYDZIEŃ
Zamówienia: 47
Przychód: 12 450,80 PLN
Średnia wartość: 265,00 PLN

📉 POPRZEDNI TYDZIEŃ
Zamówienia: 38
Przychód: 9 320,50 PLN
Średnia wartość: 245,28 PLN

📈 ZMIANA TYDZIEŃ DO TYGODNIA
Zamówienia: +23.7% ⬆️
Przychód: +33.6% ⬆️
Średnia wartość: +8.0% ⬆️

📆 DZIENNY BREAKDOWN
2025-11-24 (pon): 5 zam., 1 200,00 PLN
2025-11-25 (wt): 8 zam., 2 150,00 PLN
2025-11-26 (śr): 7 zam., 1 850,00 PLN
2025-11-27 (czw): 9 zam., 2 400,00 PLN
2025-11-28 (pt): 11 zam., 3 100,00 PLN
2025-11-29 (sob): 4 zam., 1 050,00 PLN
2025-11-30 (niedz): 3 zam., 700,80 PLN

💡 ANALIZA AI
✨ Bardzo udany tydzień! Wzrost o 33% w porównaniu do poprzedniego.

🎯 Kluczowe obserwacje:
• Najlepsze dni: piątek i czwartek
• Spadek w weekend - typowe dla B2B
• Wzrost średniej wartości zamówienia o 8%

💡 Rekomendacje:
1. Kontynuuj działania marketingowe z tego tygodnia
2. Rozważ promocje weekendowe dla zwiększenia ruchu
3. Monitoruj dostępność bestselerów
```

## 🔧 Dostosowanie

### Zmiana dnia/czasu uruchomienia

W node'zie **"Every Monday at 08:00"** zmień:
```javascript
triggerAtDay: 1,      // 1 = poniedziałek, 7 = niedziela
triggerAtHour: 8,     // Godzina (0-23)
triggerAtMinute: 0    // Minuta (0-59)
```

### Zmiana liczby pobieranych zamówień

W node'zie **"getTygodnioweZamowienia Tool"** zmień:
```javascript
limit=200  // Zwiększ jeśli masz >200 zamówień w 2 tygodnie
```

### Zmiana języka raportu

W node'zie **"AI Agent"** zmień prompt:
```
You respond ONLY IN POLISH  →  You respond ONLY IN ENGLISH
```

### Dodanie Telegram zamiast Slack

1. Usuń node **"Send to Slack"**
2. Dodaj node **"Telegram"**
3. Połącz z **"AI Agent"**
4. Skonfiguruj Telegram Bot Token

### Dostosowanie analizy AI

W node'zie **"AI Agent"** możesz dostosować prompt, aby AI skupiło się na:
- Konkretnych produktach
- Kategoriach
- Regionach
- Kanałach sprzedaży

## 🐛 Troubleshooting

### Workflow zwraca 0 zamówień dla bieżącego tygodnia

**Problem:** Jest wtorek, a workflow pokazuje 0 zamówień
**Rozwiązanie:** Sprawdź czy:
- Logika tygodnia (pon-niedz) działa poprawnie
- Timezone jest ustawiona prawidłowo
- Zamówienia faktycznie istnieją w tym okresie w Shoper

### Error: "Could not get access token"

**Problem:** Błędne credentials
**Rozwiązanie:**
- Sprawdź username/password w Shoper
- Upewnij się że użytkownik ma uprawnienia WebAPI
- Sprawdź czy shopUrl jest bez `https://`

### Brak danych dla poprzedniego tygodnia

**Problem:** Sklep dopiero startuje lub mało zamówień
**Rozwiązanie:** To normalne - AI powinien to zauważyć i nie pokazywać % zmian

### Raport nie pojawia się na Slack

**Problem:** Brak credentials lub zły kanał
**Rozwiązanie:**
- Sprawdź Slack credentials w n8n
- Upewnij się że bot ma dostęp do kanału
- Sprawdź channelId w node'zie

### Niedzielne zamówienia pokazują się jako "pon"

**Problem:** Błąd w kalkulacji `dayOfWeek`
**Rozwiązanie:** Sprawdź czy kod obsługuje `dayOfWeek === 0` (niedziela)

## 🆙 Wersja PRO

Chcesz więcej? Zobacz wersję PRO na [BaseAI.pl](https://baseai.pl):

- ✅ Raporty miesięczne z prognozami
- ✅ Analiza produktów i kategorii
- ✅ Segmentacja klientów
- ✅ Trendy sezonowe
- ✅ Integracja z Google Analytics
- ✅ Integracja z Facebook Ads
- ✅ Custom dashboardy
- ✅ Dedykowany support
- ✅ Pełna instalacja i konfiguracja

**Cena:** 497-1497 zł/mies

## 📞 Wsparcie

- 🐛 **Issues:** [GitHub Issues](https://github.com/V-Slot-poland/shoper-n8n-free/issues)
- 📖 **Dokumentacja:** [Shoper API](https://developers.shoper.pl)
- 💬 **Community:** [n8n Community](https://community.n8n.io)
- 📧 **Email:** admin@baseai.pl

---

**Made with ❤️ by [BaseAI.pl](https://baseai.pl)**
