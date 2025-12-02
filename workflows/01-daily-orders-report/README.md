# 📊 Codzienny Raport Zamówień + AI

Automatyczny workflow generujący inteligentne raporty sprzedażowe z analizą AI.

## ✨ Funkcje

- 🕐 **Automatyczne uruchamianie** o 23:58 każdego dnia
- 📊 **Pobieranie zamówień** z Shoper API (dwuetapowa autoryzacja)
- 🤖 **Analiza AI** (GPT-4o-mini) w języku polskim
- 💬 **Wysyłka raportu** na Slack/Telegram
- 💰 **Wielowalutowość** (PLN, EUR, USD)
- ⚡ **Timezone-safe** (działa poprawnie na CET/UTC)

## 📋 Wymagania

- **n8n** (self-hosted lub cloud) - [pobierz](https://n8n.io)
- **Shoper** z dostępem do WebAPI
- **OpenAI API Key** (~$0.01/dzień)
- **Slack** lub Telegram (opcjonalnie)

## 🚀 Instalacja

### Krok 1: Import workflow

1. Pobierz plik `workflow.json`
2. W n8n kliknij **"Import from File"**
3. Wybierz pobrany plik

### Krok 2: Konfiguracja Shoper API

**Utwórz użytkownika API:**

1. Panel Shoper → **Konfiguracja → Użytkownicy**
2. **Dodaj użytkownika** z uprawnieniami WebAPI
3. Zapisz login i hasło

**Wpisz credentials w workflow:**

W node'zie **"getDzisiejszeZamowienia Tool"** zamień:

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
Schedule Trigger (23:58)
    ↓
AI Agent (GPT-4o-mini)
    ↓
getDzisiejszeZamowienia Tool
    ├─ POST /auth (Basic Auth)
    ├─ GET /orders (Bearer token)
    └─ Client-side date filtering
    ↓
AI Analysis (Polish)
    ↓
Send to Slack
```

### Szczegóły techniczne

**Autoryzacja Shoper:**
- Dwuetapowa: `POST /auth` → Bearer token
- Token ważny 30 dni
- [Dokumentacja API](https://developers.shoper.pl)

**Filtrowanie zamówień:**
- Pobieramy ostatnie 50 zamówień (max limit API)
- Filtrujemy client-side: `order.date.substring(0, 10) === dateStr`
- Shoper `filters[date]` nie działa poprawnie

**Timezone handling:**
```javascript
const today = new Date();
const dateStr = today.toISOString().split('T')[0]; // timezone-safe
```

## 📸 Przykładowy raport

```
📊 Podsumowanie dnia 2025-12-02:

📈 Kluczowe metryki:
- Liczba zamówień: 11
- Łączna wartość zamówień: 3 075,27 PLN

✨ Ocena dnia: Bardzo udany dzień!
   Znaczący wzrost zamówień w porównaniu do średniej.

💡 Rekomendacje:
1. Rozważ zwiększone działania promocyjne
2. Monitoruj dostępność najpopularniejszych produktów
3. Przeanalizuj działania marketingowe z dzisiaj
```

## 🔧 Dostosowanie

### Zmiana czasu uruchomienia

W node'zie **"Daily at 23:58"** zmień:
```javascript
triggerAtHour: 23,    // Godzina (0-23)
triggerAtMinute: 58   // Minuta (0-59)
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

### Zwiększenie liczby zamówień

W node'zie **"getDzisiejszeZamowienia Tool"** zmień:
```javascript
limit=50  →  limit=50  // 50 to max, więcej wymaga paginacji
```

## 🐛 Troubleshooting

### Workflow zwraca 0 zamówień

**Problem:** Strefa czasowa
**Rozwiązanie:** Sprawdź czy używasz `toISOString().split('T')[0]` bez `setHours()`

### Error: "Could not get access token"

**Problem:** Błędne credentials
**Rozwiązanie:**
- Sprawdź username/password w Shoper
- Upewnij się że użytkownik ma uprawnienia WebAPI
- Sprawdź czy shopUrl jest bez `https://`

### Error: "Malformed auth header"

**Problem:** Używasz Basic Auth zamiast Bearer token
**Rozwiązanie:** Sprawdź czy najpierw wywołujesz `/auth`, potem używasz Bearer token

### Raport nie pojawia się na Slack

**Problem:** Brak credentials lub zły kanał
**Rozwiązanie:**
- Sprawdź Slack credentials w n8n
- Upewnij się że bot ma dostęp do kanału
- Sprawdź channelId w node'zie

## 🆙 Wersja PRO

Chcesz więcej? Zobacz wersję PRO na [BaseAI.pl](https://baseai.pl):

- ✅ Raporty tygodniowe/miesięczne
- ✅ Analiza trendów i prognoz
- ✅ Alerty spadków sprzedaży
- ✅ Integracja z Google Analytics
- ✅ Integracja z Facebook Ads
- ✅ Dedykowany support
- ✅ Pełna instalacja i konfiguracja

**Cena:** 297-997 zł

## 📞 Wsparcie

- 🐛 **Issues:** [GitHub Issues](https://github.com/V-Slot-poland/shoper-n8n-free/issues)
- 📖 **Dokumentacja:** [Shoper API](https://developers.shoper.pl)
- 💬 **Community:** [n8n Community](https://community.n8n.io)
- 📧 **Email:** admin@baseai.pl

---

**Made with ❤️ by [BaseAI.pl](https://baseai.pl)**
