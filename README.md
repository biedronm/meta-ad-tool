# 🔍 Meta Ads Bot — README

Narzędzie do analizy reklam marek i konkurencji z **Biblioteki Reklam Meta** (Facebook, Instagram, Messenger, Audience Network). Jeden plik HTML — bez instalacji, bez serwera, bez opłat. Otwierasz w przeglądarce, wklejasz token, wpisujesz marki i dostajesz analizę z wykresami, raport PDF, Excel oraz plik dla AI.

---

## Spis treści

1. [Pierwsze uruchomienie (token Meta)](#1-pierwsze-uruchomienie-token-meta)
2. [Jak zrobić analizę](#2-jak-zrobić-analizę)
3. [Ustawienia](#3-ustawienia)
4. [Wyniki — zakładki](#4-wyniki--zakładki)
5. [Lupa detektywa — skąd pochodzi każda informacja](#5-lupa-detektywa--skąd-pochodzi-każda-informacja)
6. [Eksporty: PDF, Excel, plik dla AI](#6-eksporty-pdf-excel-plik-dla-ai)
7. [Zestawy konkurentów i migawki](#7-zestawy-konkurentów-i-migawki)
8. [Słowniczek wskaźników](#8-słowniczek-wskaźników)
9. [Ograniczenia danych — przeczytaj przed wysłaniem raportu klientowi](#9-ograniczenia-danych--przeczytaj-przed-wysłaniem-raportu-klientowi)
10. [Rozwiązywanie problemów](#10-rozwiązywanie-problemów)
11. [Prywatność i bezpieczeństwo](#11-prywatność-i-bezpieczeństwo)

---

## 1. Pierwsze uruchomienie (token Meta)

Robisz to raz. Weryfikacja tożsamości może potrwać od kilku godzin do 1–2 dni.

1. **Potwierdź tożsamość** — wejdź na [facebook.com/ID](https://www.facebook.com/ID) i przejdź weryfikację (dokument + kraj). Bez tego API zwraca błąd 10, nawet z poprawnym tokenem.
2. **Zaakceptuj dostęp do API** — wejdź na [facebook.com/ads/library/api](https://www.facebook.com/ads/library/api) i kliknij „Rozpocznij”.
3. **Utwórz aplikację** — [developers.facebook.com](https://developers.facebook.com) → Moje aplikacje → Utwórz aplikację (typ „Inne” / „Firma”). Nie musisz niczego w niej konfigurować.
4. **Wygeneruj token** — [Graph API Explorer](https://developers.facebook.com/tools/explorer): wybierz aplikację → „User Token” → **Generate Access Token**. Żadnych dodatkowych uprawnień (scopes) nie trzeba.
5. **Przedłuż token do ~60 dni** — [Access Token Debugger](https://developers.facebook.com/tools/debug/accesstoken): wklej token → „Przedłuż token dostępu”.

**Szybki test** w Graph API Explorer:

```
ads_archive?search_terms=natura&ad_reached_countries=['PL']&fields=page_name&limit=5
```

Jeśli widzisz nazwy stron — wszystko działa. Ważne: weryfikacja, aplikacja i token muszą być z **tego samego konta** Facebook, a token wygeneruj **po** zakończeniu weryfikacji.

---

## 2. Jak zrobić analizę

1. Otwórz `meta-ads-bot.html` w Chrome lub Edge (dwuklik).
2. Wklej **token** (opcjonalnie zaznacz „Zapamiętaj token na tym komputerze”).
3. Wpisz **Twoją markę**. Pewniej: wklej **ID strony** albo link z Biblioteki Reklam (z `view_all_page_id=…`) — wtedy bot nie musi zgadywać strony po nazwie.
4. Wybierz **obszar emisji** — kraje (kafelki, można kilka) albo „🌍 Cały świat”.
5. Wpisz **konkurentów** — jeden w linii: sama nazwa albo `Nazwa; ID strony lub link`.
   Nie wiesz, kogo wpisać? Rozwiń **„🔎 Odkryj konkurentów po frazie”**, wpisz np. „krem z filtrem”, zaznacz reklamodawców i kliknij „Dodaj zaznaczone do konkurentów”.
6. Ustaw próg **evergreen** (ile dni emisji traktujemy jako „sprawdzoną” reklamę, domyślnie 30).
7. Kliknij **Analizuj**.

Jeśli bot trafił na złą stronę (np. fanpage zamiast oficjalnej), w zakładce „Przegląd” tej marki są przyciski z innymi znalezionymi stronami — kliknij właściwą.

---

## 3. Ustawienia

Rozwiń **„Założenia do szacowania kosztu i ustawienia”**.

| Ustawienie | Do czego służy |
| --- | --- |
| **Benchmark branżowy** | Podstawia CPM i częstotliwość dla branży (np. moda, finanse, B2B, przemysł). Mnożnik nakłada się na stawki wybranego kraju. |
| **Sezon** | Korekta stawek: Q4 / Black Friday / Święta (+25%) lub lato (−10%). |
| **CPM min/max, częstotliwość** | Możesz nadpisać ręcznie — np. realnymi stawkami z kont klientów w tej branży. Wybór benchmarku przywraca jego wartości. |
| **Pobieraj demografię, targetowanie i płatników** | Więcej danych, wolniej przy dużych markach. |
| **Pobieraj typ mediów** | Rozpoznaje wideo / grafikę (3 dodatkowe zapytania na markę). |
| **Sprawdzaj reklamy społeczne i polityczne** | Dla nich Meta podaje **prawdziwe** wydatki. |
| **Eksport / import migawek** | Kopia zapasowa historii analiz (do zakładki „Zmiany”). |
| **Wersja Graph API** | Zmień (np. na `v24.0`), jeśli pojawi się błąd wersji. |

**Filtry wyników** (pod formularzem, po analizie) przeliczają wszystko od razu, bez ponownego pobierania: okres emisji, platforma, format, tylko aktywne, próg evergreen.

---

## 4. Wyniki — zakładki

Na górze wybierasz **📊 Porównanie** albo konkretną markę (liczba obok nazwy to indeks aktywności 0–100). Niżej — zakładki tematyczne. Każda zaczyna się od ramki 💡 z najważniejszym wnioskiem.

### Zakładki marki

| Zakładka | Co zawiera |
| --- | --- |
| **Przegląd** | KPI, indeks aktywności, najważniejsze w skrócie, trend 12 miesięcy, szacowany koszt, największe aktywne reklamy |
| **Prawdopodobny budżet** | Przedziały wydatków z prawdopodobieństwem (99% / 95% / 90% / 80% / 50%), rozkład, kalkulator „jakie są szanse, że wydaje więcej niż X zł”, co najbardziej wpływa na niepewność. Własne CPM, częstotliwość i inne założenia — działają tylko w tej zakładce |
| **Kreacje i testy** | % reklam wyłączanych w ≤7 dni, reklamy działające równolegle, DCO, formaty, platformy, długość życia reklam, efektywność zasięgu, evergreeny |
| **Kampanie** | Koncepty kreatywne (ten sam tekst = jeden koncept) i fale startów (dni z ≥3 nowymi reklamami) |
| **Przekaz** | Ton (sprzedażowy / wizerunkowy), CTA, emoji, długość tekstów, najczęstsze słowa i hasła, mechaniki, strony docelowe, nagłówki |
| **Oferty i ceny** | Rabaty (średni, maksymalny, rozkład), ceny, mechaniki promocyjne, reklamy z najwyższym rabatem |
| **Odbiorcy** | Piramida wieku i płci, kraje, targetowanie deklarowane, lokalizacje i wykluczenia, płatnik i beneficjent |
| **Kalendarz** | Mapa cieplna startów z całego roku, dzień tygodnia startu, aktywność przed ważnymi datami |
| **Zmiany** | Co nowego od poprzedniej analizy (nowe i wyłączone reklamy, zmiana budżetu, nowe evergreeny) |
| **Społeczne i polityczne** | Prawdziwe wydatki i wyświetlenia, odbiorcy, regiony (tylko jeśli marka ma takie reklamy) |

### Zakładki porównania

| Zakładka | Co zawiera |
| --- | --- |
| **Wnioski i rekomendacje** | Najważniejsze wnioski, rekomendacje z priorytetami, karty „marki w skrócie” |
| **Udział w głosie** | Budżety marek z przedziałami 80% i 95%, udziały w wydatkach / reklamach / zasięgu, udział w zasięgu miesiąc po miesiącu, emitowane reklamy w czasie |
| **Kreacje i testy** | Formaty, testowanie, koncepty, DCO, evergreeny, platformy — marka obok marki |
| **Przekaz i oferty** | „Konkurencja mówi, Ty nie”, Twoje wyróżniki, oferty, rabaty, ceny, CTA |
| **Odbiorcy** | Struktura wieku, luki demograficzne (gdzie konkurencja ma przewagę), lokalizacje |
| **Kalendarz** | Tygodniowa oś startów wszystkich marek z ważnymi datami |
| **Rankingi i indeks** | Wskaźniki indeksu i rankingi z różnicą „vs Ty” |

---

## 5. Lupa detektywa — skąd pochodzi każda informacja

Pasek pod nagłówkiem strony. Zaznacz, co chcesz oznaczyć na wynikach — bloki dostaną kolorową ramkę, tło i etykietę. Oznaczenia trafiają też do PDF.

| Oznaczenie | Co to jest | Przykłady |
| --- | --- | --- |
| 🟩 **Czyste dane** | Wartości wprost z Meta API | listy reklam, zasięg, demografia, targetowanie, płatnik, formaty |
| 🟦 **Obliczenia** | Matematyka na danych | średnie, udziały, trendy, rankingi, rozkłady |
| 🟨 **Poszlaki** | Dyskusyjne: modele, przyjęte progi, wykrywanie w tekstach | szacowany koszt, indeks aktywności, rabaty, ceny, ton, CTA, koncepty |
| 🟪 **Dedukcja detektywa** | Interpretacje narzędzia | ramki 💡, „Najważniejsze w skrócie”, rekomendacje |

Przed wysłaniem raportu klientowi warto włączyć „Poszlaki” i „Dedukcję”, żeby świadomie zdecydować, co zostawić.

---

## 6. Eksporty: PDF, Excel, plik dla AI

### 📄 Raport PDF

Przycisk **Raport PDF** → panel z ustawieniami:

- tytuł, klient, autor, logo na okładkę, komentarz analityka,
- wybór sekcji w trzech grupach (Ogólne / Porównanie / Strony każdej marki) i marek,
- gotowe zestawy: **Pełny raport** (~50 stron przy 4 markach) i **Skrót dla klienta** (~10 stron).

Otworzy się okno drukowania — jako drukarkę wybierz **„Zapisz jako PDF”**. Jeśli tła się nie drukują, zaznacz „Grafika tła”.

### 📊 Excel

Przycisk **Pobierz Excel** — 18 zakładek (porównanie, podsumowanie, rekomendacje, tendencja, kreacje i testy, kampanie, kalendarz, evergreeny, copy, przekaz, rabaty i ceny, demografia, luki, targetowanie, zmiany, reklamy społeczne, wszystkie reklamy). **Same wartości, bez formuł.**

### 🤖 Eksport dla AI (.md)

Plik Markdown z gotowym promptem i danymi — do wklejenia lub załączenia w Claude, ChatGPT lub innym AI.

1. Wybierz, co AI ma przeanalizować (pozycja konkurencyjna, kreacje, komunikacja, oferty, odbiorcy, budżet, kalendarz, zmiany, reklamy społeczne, rekomendacje, brief kreatywny) albo gotowy zestaw.
2. Wybierz formę odpowiedzi: **Audyt** (analiza → hipotezy → problemy → plan), **raport dla klienta**, **notatka dla zespołu** albo **konspekt prezentacji**.
3. Uzupełnij branżę, cel marketingowy i kontekst — im więcej, tym trafniejsza analiza.
4. Ustaw liczbę tekstów reklam w pliku (więcej = lepsza analiza komunikacji, ale większy plik).
5. **Pobierz plik .md** albo **Kopiuj do schowka**. „Podgląd” pokazuje, co dokładnie trafi do AI.

Prompt każe AI opierać się wyłącznie na danych z pliku, odróżniać dane od szacunków (te same znaczniki co Lupa detektywa) i oznaczać hipotezy. Opcja „Dołącz wnioski bota” dodaje automatyczne rekomendacje do zweryfikowania przez AI.

---

## 7. Zestawy konkurentów i migawki

**Zestawy** (przyciski u góry formularza): zapisz markę, konkurentów, obszar i język pod nazwą, np. „Natura — drogerie”, i wczytuj jednym kliknięciem. „Eksport / Import” przenosi zestawy między komputerami.

**Migawki** zapisują się automatycznie po każdej analizie (jedna dziennie na stronę i obszar). Przy kolejnej analizie — najwcześniej następnego dnia — zakładka **Zmiany** pokaże różnice. Migawki żyją w przeglądarce: przed zmianą komputera lub czyszczeniem danych zrób „Eksport migawek”.

---

## 8. Słowniczek wskaźników

| Wskaźnik | Definicja |
| --- | --- |
| **Aktywna reklama** | Bez daty zakończenia emisji albo z datą od dziś wzwyż |
| **Evergreen** | Reklama emitowana co najmniej X dni (ustawiasz sam, domyślnie 30) |
| **Zasięg** | `eu_total_reach` — szacunek Meta: ile kont w UE zobaczyło reklamę |
| **Szacowany koszt** | Zasięg dzienny aktywnych reklam × częstotliwość × CPM (z benchmarku) |
| **Wyłączane ≤7 dni** | % zakończonych reklam, które działały maks. 7 dni — miara intensywności testów |
| **Aktywne naraz** | Średnia dzienna liczba równolegle działających reklam z ostatnich 90 dni |
| **DCO** | Reklamy z kilkoma wariantami tekstu (dynamiczne kreacje) |
| **Koncept** | Reklamy z tym samym tekstem głównym (różne grafiki i zestawy liczą się razem) |
| **Fala startów** | Dzień, w którym wystartowały co najmniej 3 nowe reklamy |
| **Indeks aktywności 0–100** | Skala 25%, zasięg 25%, świeżość 15%, testowanie 15%, regularność 10%, różnorodność 10% (skale logarytmiczne) |
| **Ton sprzedażowy** | Reklama zawiera ofertę, cenę lub rabat |
| **Luka demograficzna** | Różnica udziału grupy (wiek + płeć) w zasięgu: średnia konkurencji minus Twoja marka, w pp |
| **Przedział prawdopodobieństwa budżetu** | Zakres, w którym — przy przyjętych założeniach — mieści się wydatek z danym prawdopodobieństwem. Model: CPM, częstotliwość i dokładność zasięgu jako rozkłady log-normalne; podane zakresy traktowane jako przedziały ustalonej pewności (domyślnie 80%). To prawdopodobieństwo w ramach modelu, nie gwarancja |
| **Udział w głosie** | Udział marki w sumie szacowanych wydatków / aktywnych reklam / zasięgu wszystkich analizowanych marek |

---

## 9. Ograniczenia danych — przeczytaj przed wysłaniem raportu klientowi

- **Brak realnych wydatków i wyników.** Meta nie publikuje wydatków reklam komercyjnych ani CTR / konwersji / ROAS. Koszt to zawsze szacunek — nadaje się do porównań między markami, nie jako dokładny budżet. Przedziały z zakładki „Prawdopodobny budżet” pokazują niepewność tego szacunku, ale też opierają się na założeniach o CPM i częstotliwości — jeśli rzeczywiste stawki są poza zakresem, wydatek może wypaść poza przedziały.
- **Tylko reklamy, które dotarły do UE.** Kampanie emitowane wyłącznie np. w USA są niewidoczne. Zasięg i demografia dotyczą tylko UE.
- **Opóźnienie i retencja.** Reklamy pojawiają się z opóźnieniem do 1–2 dni; zakończone znikają ok. rok po końcu emisji.
- **Zasięg nie jest unikalny między reklamami.** Ta sama osoba może być policzona w kilku reklamach.
- **Wykrywanie w tekstach to reguły.** Rabaty, ceny, CTA i ton mogą się czasem pomylić przy nietypowych sformułowaniach.
- **Pełne adresy stron docelowych nie są dostępne w API** — widać domenę z linku wyświetlanego w reklamie.
- **Evergreen to poszlaka, nie dowód**, że reklama działa dobrze.
- **Rekomendacje są regułowe** — nie znają marży, celów i działań poza Meta. Traktuj je jako punkt wyjścia.

---

## 10. Rozwiązywanie problemów

| Problem | Rozwiązanie |
| --- | --- |
| **Błąd Meta 10** („Application does not have permission”) | Dokończ weryfikację na facebook.com/ID i akceptację na facebook.com/ads/library/api, potem wygeneruj **nowy** token tym samym kontem. |
| **Błąd Meta 190** | Token wygasł — wygeneruj nowy i przedłuż go w Access Token Debugger. |
| **„Limit zapytań API, czekam…”** | Normalne przy wielu markach — bot sam odczeka i pociągnie dalej. |
| **Błąd wersji API** | W ustawieniach zmień wersję Graph API, np. na `v24.0`. |
| **Bot wybrał złą stronę** | Podaj ID strony / link z Biblioteki Reklam albo kliknij właściwą stronę w zakładce „Przegląd”. |
| **Brak demografii** | Włącz „Pobieraj demografię…”; dla reklam spoza UE Meta jej nie podaje. |
| **Zakładka „Zmiany” pusta** | To pierwsza analiza tej strony na tym obszarze — porównanie pojawi się przy kolejnej (najwcześniej następnego dnia). |
| **PDF bez kolorowych teł** | W oknie drukowania zaznacz „Grafika tła”. |
| **Wolne pobieranie dużej marki** | Wyłącz „Pobieraj demografię…” i „Pobieraj typ mediów”. |

---

## 11. Prywatność i bezpieczeństwo

- Plik działa w całości w Twojej przeglądarce. Dane idą tylko między przeglądarką a Meta (graph.facebook.com). Biblioteka do Excela ładuje się z cdnjs, czcionka z Google Fonts.
- Token zapisuje się tylko lokalnie i tylko wtedy, gdy zaznaczysz „Zapamiętaj token”. **Nie wysyłaj tokena nikomu** — działa jak hasło.
- Zestawy, migawki, ustawienia i logo do PDF są w pamięci przeglądarki (localStorage) na tym komputerze.
