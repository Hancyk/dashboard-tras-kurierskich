# dashboard-tras-kurierskich
# Dashboard tras kurierskich — od faktury PDF do decyzji biznesowej

**Demo:** otwórz `portfolio-dashboard-demo.html` w przeglądarce (działa w pełni offline, bez instalacji).
**Uwaga:** wszystkie dane w tym repozytorium są w 100% syntetyczne — wygenerowane losowo na potrzeby
demonstracji, nie pochodzą z żadnej realnej faktury ani firmy.

## Problem

Firma logistyczna rozlicza się z przewoźnikiem na podstawie cyklicznych faktur PDF (co 2 tygodnie),
zawierających dziesiątki wierszy dziennych danych per trasa: liczbę zrealizowanych przesyłek w
różnych pasmach cenowych, stawki paliwowe, dopłaty i — co najważniejsze — surowy wskaźnik przesyłek
niezrealizowanych.

Ten surowy wskaźnik był **systematycznie zawyżony**: część przesyłek oznaczonych jako "nieobecność
w domu" (status NH) była tego samego dnia przekierowywana do punktu odbioru (status HN SVP) i
faktycznie doręczona — ale faktura liczyła je nadal jako niezrealizowane, zaniżając realny wynik
operacyjny.

## Podejście

1. **Ekstrakcja danych** — parsowanie wielostronicowych tabel PDF (Python) do znormalizowanej
   struktury dziennej per trasa.
2. **Rekoncyliacja logiki biznesowej** — algorytm porównujący kody statusów w opisach wierszy
   (NH vs HN SVP, z uwzględnieniem mnożników) i korygujący wskaźnik niezrealizowanych przesyłek
   do wartości odzwierciedlającej rzeczywistość operacyjną, a nie artefakt raportowania faktury.
3. **Dwa dashboardy z jednego źródła danych**:
   - wersja **finansowa** (właściciel) — pełne dane: kwoty, stawki, dopłaty, VAT,
   - wersja **operacyjna** (pracownicy) — wyłącznie stopy/km/skuteczność, **fizycznie pozbawiona**
     jakichkolwiek danych finansowych w kodzie (nie tylko ukrytych w UI) — bezpieczna do wysyłki
     mailem bez ryzyka ujawnienia stawek.
4. **Panel celów KPI** — konfigurowalne progi (target dzienny, próg bonusowy, maksymalny % błędów)
   z automatycznym oznaczeniem zielony/czerwony.
5. **Automatyzacja end-to-end** — reużywalny skrypt + szablony, żeby każdy kolejny cykl
   rozliczeniowy (co 2 tygodnie) generował oba raporty w kilka minut zamiast ręcznego przepisywania.

## Stack

- **Python** — ekstrakcja i walidacja danych z PDF, generator raportów
- **Vanilla JavaScript** — logika filtrowania, obliczeń KPI, renderowania (bez frameworków —
  celowo, żeby plik wynikowy był pojedynczym, przenośnym plikiem HTML)
- **Canvas API** — własna implementacja wykresu słupkowo-liniowego (bez zależności od
  zewnętrznych bibliotek, więc dashboard działa w 100% offline)
- **HTML/CSS** — responsywny layout, spójny system kolorów i typografii

## Efekt

- Skorygowany wskaźnik przesyłek niezrealizowanych okazał się **znacząco niższy** niż wynikało to
  z surowych danych faktury — co miało bezpośrednie znaczenie dla oceny wyniku operacyjnego wobec
  zleceniodawcy.
- Cykl przygotowania raportu skrócony z ręcznego przepisywania danych do automatycznego
  generowania (dane wejściowe → gotowe pliki HTML w kilka minut).
- Rozdzielenie danych finansowych od operacyjnych rozwiązało realny problem: bezpieczne
  udostępnianie wyników zespołowi bez ekspozycji stawek i marż.

## Zrzuty ekranu

<img width="1613" height="810" alt="Zrzut ekranu 2026-08-11 003230" src="https://github.com/user-attachments/assets/f17b22e5-da30-4210-b51b-74043b4fbe0a" />
<img width="1552" height="392" alt="Zrzut ekranu 2026-08-11 003209" src="https://github.com/user-attachments/assets/cddee07a-aedf-4d90-9d95-9edbcf49a8b2" />
<img width="1886" height="975" alt="Zrzut ekranu 2026-08-11 003146" src="https://github.com/user-attachments/assets/85f913f9-3562-42eb-9bca-c6f8d7234527" />




---
Projekt zrealizowany w całości w rozmowie z Claude (Anthropic) jako część nauki automatyzacji i
analizy danych — od ekstrakcji danych, przez logikę biznesową, po finalny, gotowy do użycia produkt.
