# 🚀 Darmowe Automatyzacje n8n dla Shoper

![License: Custom](https://img.shields.io/badge/License-Custom-orange.svg)
![Shoper API](https://img.shields.io/badge/Shoper-API%20v1-blue)
![n8n](https://img.shields.io/badge/n8n-Compatible-orange)
![Made by BaseAI](https://img.shields.io/badge/Made%20by-BaseAI.pl-blueviolet)

Profesjonalne workflow n8n do automatyzacji Twojego sklepu Shoper - **całkowicie za darmo**.

Stworzone przez ekspertów z [BaseAI.pl](https://baseai.pl) - specjalistów od automatyzacji e-commerce.

> 💡 **To są przykładowe, proste workflow** pokazujące możliwości automatyzacji Shoper + n8n.
>
> Potrzebujesz czegoś bardziej zaawansowanego? [Skontaktuj się z nami](https://baseai.pl) - tworzymy dedykowane rozwiązania!

---

## 🎯 Trzy sposoby wykorzystania

### 1️⃣ Samodzielnie (FREE)
- ✅ Pobierz workflow z tego repo
- ✅ Zainstaluj na własnym n8n
- ✅ Skonfiguruj według instrukcji
- ✅ **Koszt:** 0 zł + koszty serwera

### 2️⃣ U nas - gotowe rozwiązanie (MANAGED)
**Nie masz n8n? Nie chcesz się bawić w instalację?**

Oferujemy gotowe rozwiązanie:
- ☁️ **Uruchamiamy workflow u nas** (na naszej infrastrukturze BaseAI.pl)
- 🛠️ **Wdrażamy i konfigurujemy** za Ciebie
- 📊 **Dostajesz raporty** bez instalacji czegokolwiek
- 🔧 **Dbamy o aktualizacje** i poprawki
- 💬 **Dedykowany support** email/telefon

**Cena:** od 99 zł/miesiąc (w zależności od liczby workflow)

### 3️⃣ Dedykowane rozwiązania (PRO)
**Potrzebujesz czegoś więcej niż podstawowe flow?**

Tworzymy zaawansowane automatyzacje:
- 🎯 Dedykowane workflow pod Twoje potrzeby
- 🔗 Integracje z wieloma systemami (Allegro, BaseLinker, Facebook Ads)
- 📊 Zaawansowana analityka i raporty
- 🤖 AI dostosowane do Twojej branży
- 💼 Pełne SLA i wsparcie

**Cena:** od 997 zł (jednorazowo) lub abonament

👉 **[Umów bezpłatną konsultację](https://baseai.pl)**

---

## 📦 Dostępne Workflow

### 1️⃣ [Codzienny Raport Zamówień + AI](workflows/01-daily-orders-report/)

**Status:** ✅ Gotowe do użycia

Automatyczny raport sprzedażowy z inteligentną analizą AI w języku polskim.

**Funkcje:**
- 🕐 Automatycznie o 23:58 każdego dnia
- 📊 Pobiera dzisiejsze zamówienia z Shoper API
- 🤖 AI (GPT-4o-mini) analizuje i tworzy raport po polsku
- 💬 Wysyła raport na Slack/Telegram
- 💰 Obsługa wielu walut (PLN, EUR, USD)

**[📖 Pełna dokumentacja →](workflows/01-daily-orders-report/)**

---

### 2️⃣ Webhook Shoper → Google Sheets

**Status:** 🚧 W przygotowaniu

Automatyczny zapis wszystkich zamówień do Google Sheets w czasie rzeczywistym.

**Planowane funkcje:**
- ⚡ Instant webhook przy nowym zamówieniu
- 📊 Automatyczny zapis do arkusza
- 🔄 Aktualizacja statusu zamówienia
- 📈 Gotowe wykresy i pivoty

**Dostępne wkrótce!**

---

### 3️⃣ Alert o Nowym Zamówieniu

**Status:** 🚧 W przygotowaniu

Natychmiastowe powiadomienie na Telegram przy każdym nowym zamówieniu.

**Planowane funkcje:**
- 🔔 Instant notification na Telegram
- 📦 Szczegóły zamówienia i klienta
- 💰 Wartość zamówienia
- 📍 Status płatności i wysyłki

**Dostępne wkrótce!**

---

### 4️⃣ Tygodniowy Raport Sprzedaży

**Status:** 🚧 W przygotowaniu

Kompleksowe podsumowanie tygodnia z analizą trendów.

**Planowane funkcje:**
- 📊 Porównanie tydzień do tygodnia
- 📈 Wykresy trendów sprzedaży
- 🏆 Top produkty i kategorie
- 💡 Rekomendacje AI

**Dostępne wkrótce!**

---

## 🎯 Dla kogo?

To repozytorium jest dla właścicieli sklepów Shoper, którzy chcą:

- ✅ Automatyzować codzienne operacje
- ✅ Oszczędzać czas na ręcznych czynnościach
- ✅ Mieć inteligentne analizy AI bez kodowania
- ✅ Otrzymywać alerty i raporty na Slack/Telegram
- ✅ Zacząć przygodę z automatyzacją za darmo

---

## 🚀 Szybki Start

### 1. Zainstaluj n8n

**Self-hosted (Raspberry Pi, VPS):**
```bash
npm install -g n8n
n8n start
```

**Docker:**
```bash
docker run -d --name n8n -p 5678:5678 n8nio/n8n
```

**Cloud:** [n8n.cloud](https://n8n.cloud)

### 2. Pobierz workflow

```bash
git clone https://github.com/V-Slot-poland/shoper-n8n-free.git
cd shoper-n8n-free/workflows
```

### 3. Wybierz workflow

Przejdź do wybranego katalogu i postępuj według instrukcji w README:

- [`01-daily-orders-report/`](workflows/01-daily-orders-report/) - Codzienny raport
- `02-webhook-to-sheets/` - Webhook → Google Sheets (wkrótce)
- `03-new-order-alert/` - Alert o zamówieniu (wkrótce)
- `04-weekly-report/` - Raport tygodniowy (wkrótce)

---

## 📖 Wymagania

### Dla wszystkich workflow:

- **n8n** (self-hosted lub cloud)
- **Konto Shoper** z dostępem do WebAPI

### Dla workflow z AI:

- **OpenAI API Key** (GPT-4o-mini ~$0.01-0.10/dzień)

### Dla integracji:

- **Slack** lub **Telegram** (do powiadomień)
- **Google Account** (do Google Sheets)

---

## 🆚 FREE vs PRO

| Kategoria | Free (GitHub) | PRO (BaseAI.pl) |
|-----------|---------------|-----------------|
| **Raporty** | | |
| Codzienny raport zamówień | ✅ | ✅ |
| Raport tygodniowy | ❌ | ✅ |
| Raport miesięczny | ❌ | ✅ |
| Analiza trendów | ❌ | ✅ |
| Prognozy sprzedaży | ❌ | ✅ |
| **AI** | | |
| Podstawowa analiza (GPT-4o-mini) | ✅ | ✅ |
| Zaawansowana analiza (GPT-4) | ❌ | ✅ |
| Rekomendacje personalizowane | ❌ | ✅ |
| **Alerty** | | |
| Alert o nowym zamówieniu | ✅ | ✅ |
| Alert spadku sprzedaży | ❌ | ✅ |
| Alert niskiego stanu magazynowego | ❌ | ✅ |
| **Integracje** | | |
| Slack | ✅ | ✅ |
| Telegram | ✅ | ✅ |
| Google Sheets | ✅ | ✅ |
| Google Analytics | ❌ | ✅ |
| Facebook Ads | ❌ | ✅ |
| BaseLinker | ❌ | ✅ |
| Allegro | ❌ | ✅ |
| **Support** | | |
| GitHub Issues | ✅ | ✅ |
| Dedykowany support email | ❌ | ✅ |
| Pełna instalacja i konfiguracja | ❌ | ✅ |
| Dostosowanie do firmy | ❌ | ✅ |

**Cena PRO:** 297-997 zł (jednorazowo lub abonament)

👉 **[Zobacz wersję PRO na BaseAI.pl](https://baseai.pl/automatyzacje-shoper)**

---

## 🤝 Potrzebujesz pomocy?

### Wersja FREE (GitHub)

- 📖 [Dokumentacja Shoper API](https://developers.shoper.pl)
- 💬 [Forum n8n Community](https://community.n8n.io)
- 🐛 [GitHub Issues](https://github.com/V-Slot-poland/shoper-n8n-free/issues)
- ⭐ Daj gwiazdkę jeśli projekt Ci pomógł!

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

## 🤝 Współpraca

Chcesz pomóc w rozwoju projektu?

1. Fork tego repozytorium
2. Stwórz branch (`git checkout -b feature/amazing-feature`)
3. Commit zmian (`git commit -m 'Add amazing feature'`)
4. Push do brancha (`git push origin feature/amazing-feature`)
5. Otwórz Pull Request

Zobacz [CONTRIBUTING.md](CONTRIBUTING.md) po więcej szczegółów.

---

## 📝 Licencja

**Custom License** - Użytek własny dozwolony, sprzedaż zakazana.

✅ **Możesz:**
- Używać w swoim biznesie
- Modyfikować dla własnych potrzeb
- Uczyć się i eksperymentować
- Dzielić się wiedzą

❌ **Nie możesz:**
- Sprzedawać tego kodu
- Oferować jako płatna usługa
- Usuwać informacji o autorze

Pełna treść: [LICENSE](LICENSE)

---

## 🌟 O autorze

**Damian Mazurek** | [BaseAI.pl](https://baseai.pl)
**TechnOVO Sp. z o.o.** | NIP: 8961596096
📧 admin@baseai.pl

### Specjalizacja

- 🤖 Automatyzacje n8n dla e-commerce
- 🧠 Integracje AI (GPT-4, Claude, Gemini)
- 📊 Analityka sprzedaży i marketingu
- 🔗 Łączenie systemów (Shoper, BaseLinker, Allegro, etc.)

### Zrealizowane projekty

- 50+ automatyzacji dla sklepów online
- Integracje z Shoper, BaseLinker, Allegro, WooCommerce
- AI chatboty dla obsługi klienta
- Zaawansowane dashboardy sprzedażowe

### Technologie

- **Automation:** n8n, Make, Zapier
- **AI:** OpenAI GPT-4, Claude, Gemini
- **Backend:** Python, Node.js, TypeScript
- **Data:** PostgreSQL, Redis, BigQuery

---

## 🎁 Roadmap

Planowane kolejne darmowe workflow:

- [ ] **Webhook → Google Sheets** (Q1 2025)
- [ ] **Alert o nowym zamówieniu** (Q1 2025)
- [ ] **Tygodniowy raport** (Q2 2025)
- [ ] **Automatyczne odpowiedzi email** (Q2 2025)
- [ ] **Synchronizacja stanów magazynowych** (Q2 2025)

⭐ **Daj gwiazdkę**, aby być na bieżąco!

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
