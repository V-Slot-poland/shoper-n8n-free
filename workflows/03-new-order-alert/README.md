# 🔔 Alert o Nowym Zamówieniu

Natychmiastowy alert na Telegram przy każdym nowym zamówieniu w sklepie Shoper.

## ✨ Funkcje

- 🔄 **Automatyczne sprawdzanie** co 5 minut
- 📱 **Instant alert** na Telegram
- 📊 **Szczegółowe informacje** o zamówieniu:
  - Numer zamówienia i kod
  - Wartość zamówienia
  - Status płatności (opłacone/nieopłacone)
  - Dane klienta (imię, nazwisko, firma)
  - Kontakt (email, telefon)
  - Adres dostawy
- 🔗 **Link** do zamówienia w panelu Shoper
- 💾 **Inteligentne wykrywanie** - wysyła tylko NOWE zamówienia
- ⚡ **Zero konfiguracji bazy danych** - używa n8n Static Data

## 📋 Wymagania

- n8n (self-hosted lub cloud)
- Sklep Shoper z aktywnym WebAPI
- Bot Telegram (instrukcja poniżej)
- Chat ID Telegram (instrukcja poniżej)

## 🚀 Instalacja

### Krok 1: Przygotuj Telegram Bot

1. **Utwórz bota przez BotFather:**
   - Otwórz Telegram i wyszukaj `@BotFather`
   - Wyślij komendę `/newbot`
   - Podaj nazwę bota (np. "Shoper Alerts")
   - Podaj username bota (np. "MyShopOrdersBot")
   - Zapisz **token** bota (np. `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`)

2. **Znajdź swój Chat ID:**
   - Napisz wiadomość do swojego bota
   - Otwórz w przeglądarce:
     ```
     https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates
     ```
   - Znajdź `"chat":{"id":123456789}` i zapisz **Chat ID**

### Krok 2: Przygotuj credentials Shoper

1. Zaloguj się do panelu Shoper
2. Przejdź do: **Konfiguracja → Użytkownicy**
3. Utwórz nowego użytkownika z uprawnieniami do API
4. Zapisz:
   - Adres sklepu (np. `twoj-sklep.shoper.pl`)
   - Login użytkownika
   - Hasło użytkownika

### Krok 3: Importuj workflow do n8n

1. **Zaloguj się do n8n**
2. **Kliknij "Add workflow" → "Import from File"**
3. **Wybierz plik:** `workflow.json` z tego katalogu
4. **Workflow zostanie zaimportowany**

### Krok 4: Skonfiguruj credentials

#### A) Telegram Bot

1. W workflow znajdź node **"Send to Telegram"**
2. Kliknij na node
3. W sekcji **Credentials** kliknij **"Create New"**
4. Wybierz **"Telegram API"**
5. Wklej **Bot Token** z kroku 1
6. Kliknij **"Save"**
7. W polu **"Chat ID"** wpisz swój **Chat ID** z kroku 1

#### B) Shoper API

1. W workflow znajdź node **"Get Orders from Shoper"**
2. Kliknij na node
3. W sekcji **Code** znajdź linie:
   ```javascript
   const shopUrl = 'YOUR_SHOP.shoper.pl';
   const username = 'YOUR_SHOPER_USERNAME';
   const password = 'YOUR_SHOPER_PASSWORD';
   ```
4. **Zamień placeholdery** na swoje dane:
   ```javascript
   const shopUrl = 'twoj-sklep.shoper.pl';
   const username = 'api_user';
   const password = 'twoje_haslo';
   ```

### Krok 5: Aktywuj workflow

1. **Zapisz workflow** (Ctrl+S lub przycisk "Save")
2. **Aktywuj workflow** przełączając przełącznik na **"Active"** (w prawym górnym rogu)
3. Workflow będzie automatycznie sprawdzać nowe zamówienia **co 5 minut**

### Krok 6: Test workflow

1. **Złóż testowe zamówienie** w swoim sklepie Shoper
2. **Poczekaj maksymalnie 5 minut** (lub wykonaj workflow ręcznie - przycisk "Execute Workflow")
3. **Sprawdź Telegram** - powinieneś otrzymać alert

## 📖 Jak to działa?

### Architektura workflow

```
┌─────────────────┐
│  Every 5 Min    │  ← Automatyczny trigger
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Get Orders     │  ← Pobiera zamówienia z Shoper API
│  from Shoper    │     (Basic Authentication)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Filter New     │  ← Filtruje TYLKO nowe zamówienia
│  Orders         │     (porównanie z lastOrderId)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Format         │  ← Tworzy czytelny komunikat
│  Telegram       │     z wszystkimi szczegółami
│  Message        │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Send to        │  ← Wysyła alert na Telegram
│  Telegram       │
└─────────────────┘
```

### Wykrywanie nowych zamówień

Workflow używa **n8n Static Data** do przechowywania ID ostatnio sprawdzonego zamówienia (`lastOrderId`).

**Przy każdym uruchomieniu:**
1. Pobiera zamówienia z Shoper API (`GET /webapi/rest/orders?limit=50`)
2. Porównuje `order_id` każdego zamówienia z zapisanym `lastOrderId`
3. Jeśli `order_id > lastOrderId` → to NOWE zamówienie
4. Aktualizuje `lastOrderId` na najwyższy `order_id` ze znalezionych zamówień
5. Wysyła alert tylko dla nowych zamówień

**Pierwsze uruchomienie:**
- `lastOrderId` = 0
- Wszystkie zamówienia z ostatnich 50 będą uznane za "nowe"
- Po pierwszym uruchomieniu workflow będzie wysyłać alerty tylko dla rzeczywiście nowych zamówień

### Szczegóły techniczne

**Autoryzacja Shoper API:**
- Metoda: **Basic Authentication**
- Endpoint: `GET /webapi/rest/orders?limit=50`
- Header: `Authorization: Basic <base64(username:password)>`

**Telegram API:**
- Metoda: **Bot API**
- Format: **Markdown**
- Emoji dla lepszej czytelności

## 📸 Przykład alertu

```
🔔 **NOWE ZAMÓWIENIE**

📋 **Zamówienie:** #12345 (ORD-2025-001)
📅 **Data:** 2025-12-02 14:30:15
💰 **Wartość:** 299.99 PLN
💳 **Opłacone:** ✅ TAK

👤 **Klient:**
   Imię i nazwisko: Jan Kowalski
   Firma: ACME Corp
   Email: jan.kowalski@example.com
   Telefon: +48 123 456 789
   Miasto: Warszawa

📦 **Dostawa:**
   Miasto: Warszawa
   Adres: ul. Przykładowa 123

🔗 [Otwórz w panelu](https://twoj-sklep.shoper.pl/admin/orders/details/12345)
```

## 🔧 Dostosowanie

### Zmiana częstotliwości sprawdzania

W node **"Every 5 Minutes"**:
1. Kliknij na node
2. Zmień wartość **"Minutes"** na dowolną (np. 1, 3, 10)
3. Zapisz workflow

**Uwaga:** Zbyt częste sprawdzanie może przekroczyć limity API Shoper (60 zapytań/minutę dla planu Business).

### Dodatkowe kanały powiadomień

Możesz wysyłać alerty również na:
- **Slack** - dodaj node "Slack"
- **Email** - dodaj node "Send Email"
- **SMS** - dodaj node z integracją Twilio/SMSApi
- **Discord** - dodaj node "Discord"

Podłącz nowy node po node **"Format Telegram Message"**.

### Filtrowanie zamówień

Jeśli chcesz otrzymywać alerty tylko dla określonych zamówień (np. wartość > 500 PLN):

W node **"Filter New Orders"** dodaj warunek:
```javascript
const newOrders = orders.filter(order => {
  const orderId = order.json.order_id || 0;
  const sum = order.json.sum || 0;
  return orderId > lastOrderId && sum > 500; // Tylko > 500 PLN
});
```

### Zmiana formatu wiadomości

W node **"Format Telegram Message"** możesz dostosować treść wiadomości:
- Dodać/usunąć pola
- Zmienić emoji
- Zmienić format (np. HTML zamiast Markdown)

## 🐛 Troubleshooting

### Problem: Nie otrzymuję alertów

**Rozwiązanie:**
1. Sprawdź czy workflow jest **aktywny** (zielony przełącznik w prawym górnym rogu)
2. Sprawdź **logi wykonań** workflow (zakładka "Executions")
3. Sprawdź czy **Chat ID** w Telegram jest poprawny
4. Sprawdź czy bot ma uprawnienia do wysyłania wiadomości

### Problem: Otrzymuję alert dla każdego zamówienia przy każdym sprawdzeniu

**Rozwiązanie:**
1. Node **"Filter New Orders"** nie działa poprawnie
2. Sprawdź czy `this.getWorkflowStaticData('global')` działa
3. Upewnij się, że workflow jest zapisany po pierwszym uruchomieniu

### Problem: Błąd autoryzacji Shoper API (401)

**Rozwiązanie:**
1. Sprawdź czy **username** i **password** są poprawne
2. Sprawdź czy użytkownik ma **uprawnienia do API** w panelu Shoper
3. Sprawdź czy sklep ma **aktywny plan** z dostępem do WebAPI

### Problem: Przekroczenie limitu API (429)

**Rozwiązanie:**
1. **Zwiększ interwał** sprawdzania (np. z 5 do 10 minut)
2. Sprawdź **limity dla Twojego planu** Shoper:
   - Start: 30 zapytań/minutę
   - Business: 60 zapytań/minutę
   - Premium: 120 zapytań/minutę

### Problem: Nie widzę niektórych zamówień

**Rozwiązanie:**
1. Workflow pobiera **maksymalnie 50 ostatnich zamówień**
2. Jeśli masz więcej niż 50 zamówień w ciągu 5 minut, niektóre mogą zostać pominięte
3. **Zmniejsz interwał** sprawdzania lub **zwiększ limit** w API call:
   ```javascript
   url: `https://${shopUrl}/webapi/rest/orders?limit=100`
   ```

## 💡 Porady

### Testowanie przed aktywacją

1. **Wyłącz auto-trigger:** Wyłącz node "Every 5 Minutes" (prawy klick → Disable)
2. **Dodaj Manual Trigger:** Dodaj node "Manual Trigger" i podłącz go do "Get Orders from Shoper"
3. **Testuj ręcznie:** Klikaj "Execute Workflow" aby testować
4. **Aktywuj auto-trigger:** Gdy wszystko działa, włącz z powrotem "Every 5 Minutes"

### Monitoring alertów

Możesz dodać dodatkowy node, który:
- Zapisuje alerty do Google Sheets (historia zamówień)
- Wysyła raport dzienny z liczbą nowych zamówień
- Integruje się z systemem CRM

### Bezpieczeństwo

⚠️ **WAŻNE:**
- **NIE udostępniaj** pliku `workflow.json` z credentials!
- **Używaj zmiennych środowiskowych** lub n8n Credentials dla danych wrażliwych
- **Regularnie zmieniaj** hasła użytkowników API
- **Ogranicz uprawnienia** użytkownika API tylko do odczytu zamówień

## 🆙 Wersja PRO

Chcesz więcej? Sprawdź naszą **wersję PRO** na [BaseAI.pl](https://baseai.pl):

### MANAGED (Hosting u nas)
- 🏆 Gotowe środowisko n8n
- 🔒 Bezpieczne przechowywanie credentials
- 📊 Dashboard z metrykami
- 💬 Wsparcie techniczne
- 🚀 Aktualizacje workflow

**Od 99 PLN/miesiąc** → [Zamów hosting](https://baseai.pl/managed)

### PRO (Dedykowane rozwiązania)
- 🎯 Workflow dopasowane do Twojego biznesu
- 🤖 Zaawansowana automatyzacja AI
- 📈 Analityka i raporty
- 🔗 Integracje z CRM, ERP, etc.
- 👨‍💻 Dedykowany support

**Od 2000 PLN** → [Skontaktuj się](https://baseai.pl/contact)

## 📞 Wsparcie

### Darmowe workflow (GitHub)
- 📖 [Dokumentacja](https://github.com/V-Slot-poland/shoper-n8n-free)
- 💬 [GitHub Issues](https://github.com/V-Slot-poland/shoper-n8n-free/issues)
- 🌐 [Forum społeczności](https://github.com/V-Slot-poland/shoper-n8n-free/discussions)

### Wsparcie komercyjne
- 📧 Email: admin@baseai.pl
- 🌐 Website: [BaseAI.pl](https://baseai.pl)
- 💼 LinkedIn: [BaseAI](https://linkedin.com/company/baseai)

## 📄 Licencja

**Custom License** - Użytek własny dozwolony, sprzedaż zakazana.

Szczegóły: [LICENSE](../../LICENSE)

---

## 🙏 Podziękowania

Workflow powstało w ramach projektu **Shoper n8n Free Workflows** by [BaseAI.pl](https://baseai.pl).

Jeśli ten workflow Ci pomógł:
- ⭐ **Zostaw gwiazdkę** na [GitHub](https://github.com/V-Slot-poland/shoper-n8n-free)
- 📢 **Podziel się** z innymi właścicielami sklepów Shoper
- 💬 **Daj znać** co można ulepszyć

---

**Made with ❤️ by [BaseAI.pl](https://baseai.pl)**

**Autor:** Damian Mazurek | **Email:** admin@baseai.pl | **NIP:** 8961596096
