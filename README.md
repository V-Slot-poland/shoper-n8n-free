# 🚀 Darmowe Automatyzacje n8n dla Shoper

![License: Custom](https://img.shields.io/badge/License-Custom-orange.svg)
![Shoper API](https://img.shields.io/badge/Shoper-API%20v1-blue)
![n8n](https://img.shields.io/badge/n8n-Compatible-orange)
![Made by BaseAI](https://img.shields.io/badge/Made%20by-BaseAI.pl-blueviolet)

Profesjonalne workflow n8n do automatyzacji Twojego sklepu Shoper - **całkowicie za darmo**.

Stworzone przez ekspertów z [BaseAI.pl](https://baseai.pl) - specjalistów od automatyzacji e-commerce.

---

## 📦 Co znajdziesz w tym repozytorium?

### ✅ Flow #1: Codzienny Raport Zamówień + AI

**Co robi:**
- 🕐 Automatycznie o 23:58 każdego dnia
- 📊 Pobiera dzisiejsze zamówienia z Shoper API
- 🤖 AI (GPT-4o-mini) analizuje sprzedaż i tworzy inteligentny raport po polsku
- 💬 Wysyła raport na Slack (lub dowolny kanał komunikacji)

**Funkcje:**
- ✅ Liczba zamówień
- ✅ Łączna wartość sprzedaży
- ✅ Lista pierwszych 10 zamówień z klientami
- ✅ Inteligentna analiza AI w języku polskim
- ✅ Emoji dla lepszej czytelności
- ✅ Obsługa wielu walut (PLN, EUR, USD)

**Przykładowy raport:**
```
📊 Podsumowanie dnia 2025-12-02:

📈 Kluczowe metryki:
- Liczba zamówień: 11
- Łączna wartość zamówień: 3 075,27 PLN

✨ Ocena dnia: Dzień bardzo udany! Znaczący wzrost zamówień.

💡 Rekomendacje:
1. Rozważ zwiększone działania promocyjne
2. Monitoruj dostępność najpopularniejszych produktów
```

---

## 🎯 Dla kogo?

To repozytorium jest dla właścicieli sklepów Shoper, którzy chcą:

- ✅ Automatyzować codzienne raporty sprzedaży
- ✅ Oszczędzać czas na ręcznym sprawdzaniu zamówień
- ✅ Mieć inteligentne analizy AI bez kodowania
- ✅ Otrzymywać raporty na Slack/Telegram
- ✅ Zacząć przygodę z automatyzacją za darmo

---

## 🚀 Instalacja

### Wymagania

- **n8n** (self-hosted lub cloud) - [Pobierz tutaj](https://n8n.io)
- **Konto Shoper** z dostępem do API
- **OpenAI API Key** (GPT-4o-mini ~$0.01/dzień)
- **Slack** (opcjonalnie, można zastąpić Telegram/Email)

### Krok 1: Pobierz workflow

```bash
git clone https://github.com/V-Slot-poland/shoper-n8n-free.git
cd shoper-n8n-free
```

### Krok 2: Zaimportuj do n8n

1. Otwórz n8n
2. Kliknij **"Import from File"**
3. Wybierz plik: `daily-orders-report.json`
4. Workflow zostanie zaimportowany

### Krok 3: Konfiguracja Shoper API

**Utwórz użytkownika API w Shoper:**

1. Zaloguj się do panelu Shoper
2. Przejdź do: **Konfiguracja → Użytkownicy → Dodaj użytkownika**
3. Nadaj uprawnienia do **WebAPI**
4. Zapisz **login** i **hasło**

**Wpisz dane w workflow:**

W node'zie **"getDzisiejszeZamowienia Tool"** zamień:

```javascript
const shopUrl = 'YOUR_SHOP.shoper.pl';      // Twój sklep
const username = 'YOUR_SHOPER_USERNAME';    // Login użytkownika API
const password = 'YOUR_SHOPER_PASSWORD';    // Hasło użytkownika
```

### Krok 4: Konfiguracja OpenAI

1. W n8n przejdź do **Credentials → Add Credential**
2. Wybierz **"OpenAI"**
3. Wklej swój **API Key** z [platform.openai.com](https://platform.openai.com/api-keys)
4. Zapisz

### Krok 5: Konfiguracja Slack

1. W n8n dodaj **Slack Credentials**
2. Autoryzuj aplikację
3. Wybierz kanał w node'zie **"Send to Slack"**

### Krok 6: Testuj!

1. Kliknij **"Execute workflow"**
2. Sprawdź czy raport pojawił się na Slack
3. Gotowe! Workflow będzie działać automatycznie o 23:58

---

## 📖 Jak działa?

### Architektura workflow

```
┌─────────────────┐
│  Trigger        │  Daily at 23:58
│  (Schedule)     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  AI Agent       │  Orchestrates the flow
│  (GPT-4o-mini)  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Tool: Fetch    │  1. POST /auth → get access_token
│  Orders         │  2. GET /orders → fetch today's orders
│                 │  3. Client-side filtering by date
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  AI Analysis    │  Creates Polish summary with:
│                 │  - Metrics, recommendations, insights
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Send to Slack  │  Formatted report
└─────────────────┘
```

### Kluczowe szczegóły techniczne

**Autoryzacja Shoper API:**
- Dwuetapowa: `POST /auth` (Basic Auth) → Bearer token
- Token ważny 30 dni
- Dokumentacja: [developers.shoper.pl](https://developers.shoper.pl)

**Filtrowanie zamówień:**
- API Shoper nie wspiera `filters[date]` poprawnie
- Rozwiązanie: client-side filtering po `substring(0, 10)`
- Pobieramy ostatnie 50 zamówień (max API limit)

**Timezone handling:**
- `new Date().toISOString().split('T')[0]` - timezone-safe
- Działa poprawnie na CET/UTC bez przesunięć

---

## 🆚 FREE vs PRO

| Funkcja | Free (GitHub) | PRO (BaseAI.pl) |
|---------|---------------|-----------------|
| Codzienny raport zamówień | ✅ | ✅ |
| Analiza AI w języku polskim | ✅ | ✅✅ (GPT-4) |
| Integracja Slack | ✅ | ✅ |
| Integracja Telegram | ❌ | ✅ |
| Raporty tygodniowe/miesięczne | ❌ | ✅ |
| Analiza trendów sprzedaży | ❌ | ✅ |
| Alerty spadków sprzedaży | ❌ | ✅ |
| Raport magazynowy | ❌ | ✅ |
| Integracja z Google Analytics | ❌ | ✅ |
| Integracja z Facebook Ads | ❌ | ✅ |
| Pełna instalacja + konfiguracja | ❌ | ✅ |
| Dedykowany support | ❌ | ✅ |
| Dostosowanie do Twojej firmy | ❌ | ✅ |

**Cena PRO:** 297-997 zł (w zależności od pakietu)

👉 **[Zobacz wersję PRO na BaseAI.pl](https://baseai.pl/automatyzacje-shoper)**

---

## 🤝 Potrzebujesz pomocy?

### Wersja FREE (GitHub)

- 📖 [Dokumentacja Shoper API](https://developers.shoper.pl)
- 💬 [Forum n8n Community](https://community.n8n.io)
- ⭐ Daj gwiazdkę na GitHub jeśli projekt Ci pomógł!

### Wersja PRO + Wsparcie

Potrzebujesz:
- ✅ Profesjonalnego wdrożenia?
- ✅ Dedykowanych automatyzacji?
- ✅ Integracji z wieloma systemami?
- ✅ Supportu i konsultacji?

**Skontaktuj się z nami:**
- 🌐 Website: [baseai.pl](https://baseai.pl)
- 📧 Email: admin@baseai.pl
- 💼 LinkedIn: [BaseAI](https://linkedin.com/company/baseai-pl)

---

## 📝 Licencja

**Custom License** - Użytek własny dozwolony, sprzedaż zakazana.

✅ Możesz: używać w swoim biznesie, modyfikować, uczyć się
❌ Nie możesz: sprzedawać, oferować jako płatna usługa

Pełna treść: [LICENSE](LICENSE)

---

## 🌟 O autorze

**Damian Mazurek** | [BaseAI.pl](https://baseai.pl)
**TechnOVO Sp. z o.o.** | NIP: 8961596096
📧 admin@baseai.pl

Specjalizujemy się w:
- 🤖 Automatyzacje n8n dla sklepów online
- 🧠 Integracje AI (GPT-4, Claude, Gemini)
- 📊 Analityka sprzedaży i marketingu
- 🔗 Łączenie systemów (Shoper, BaseLinker, Allegro, etc.)

**Zrealizowane projekty:**
- 50+ automatyzacji dla e-commerce
- Integracje z Shoper, BaseLinker, Allegro, WooCommerce
- AI chatboty dla obsługi klienta
- Zaawansowane reportingi sprzedażowe

**Technologie:**
- n8n, Make, Zapier
- OpenAI GPT-4, Claude, Gemini
- Python, Node.js, TypeScript
- PostgreSQL, Redis

---

## 🎁 Więcej darmowych flow (wkrótce)

Planowane kolejne darmowe workflow:

- 📦 **Webhook Shoper → Google Sheets** - automatyczny zapis zamówień
- 🔔 **Alert o nowym zamówieniu** - instant notification na Telegram
- 📧 **Auto-odpowiedzi email** - inteligentne odpowiedzi AI dla klientów
- 📊 **Tygodniowy raport sprzedaży** - podsumowanie tygodnia

⭐ **Daj gwiazdkę**, aby być na bieżąco z nowymi flow!

---

## 🚀 Made with ❤️ by [BaseAI.pl](https://baseai.pl)

Automatyzujemy e-commerce, żebyś Ty mógł skupić się na rozwoju biznesu.

**[👉 Zobacz co możemy dla Ciebie zrobić](https://baseai.pl)**

---

## 📊 Stats

![GitHub stars](https://img.shields.io/github/stars/V-Slot-poland/shoper-n8n-free?style=social)
![GitHub forks](https://img.shields.io/github/forks/V-Slot-poland/shoper-n8n-free?style=social)
![GitHub watchers](https://img.shields.io/github/watchers/V-Slot-poland/shoper-n8n-free?style=social)

*Jeśli ten projekt jest dla Ciebie wartościowy, rozważ wersję PRO i wesprzyj dalszy rozwój automatyzacji dla polskiego e-commerce!*
