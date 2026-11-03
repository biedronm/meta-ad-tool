#  https://biedronm.github.io/meta-ad-tool/

# Meta Ads Bot — README

Narzędzie do analizy reklam marek i konkurencji z **Biblioteki Reklam Meta** (Facebook, Instagram, Messenger, Audience Network). Jeden plik HTML — bez instalacji, bez serwera, bez opłat. Otwierasz w przeglądarce, wklejasz token, wpisujesz marki i dostajesz analizę z wykresami, raport PDF, Excel oraz plik dla AI.

---

## Spis treści

1. [Pierwsze uruchomienie (token Meta)](#1-pierwsze-uruchomienie-token-meta)
2. [Jak zrobić analizę](#2-jak-zrobić-analizę)
3. [Ustawienia](#3-ustawienia)
4. [Wyniki — zakładki](#4-wyniki--zakładki)
5. [Lupa detektywa — skąd pochodzi każda informacja](#5-lupa-detektywa--skąd-pochodzi-każda-informacja)
6. [Eksporty: PDF, Excel, plik dla AI, pakiet do strategii](#6-eksporty-pdf-excel-plik-dla-ai-pakiet-do-strategii)
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
4. Wybierz **obszar emisji** — kraje (kafelki, można kilka) albo „Cały świat”.
5. Wpisz **konkurentów** — jeden w linii: sama nazwa albo `Nazwa; ID strony lub link`.
   Nie wiesz, kogo wpisać? Rozwiń **„Odkryj konkurentów po frazie”**, wpisz np. „krem z filtrem”, zaznacz reklamodawców i kliknij „Dodaj zaznaczone do konkurentów”.
6. Pozostałe ustawienia są schowane pod **„Ustawienia zaawansowane”** (zwinięty pasek pokazuje ich aktualne wartości, np. „evergreen ≥30 dni · bez limitu reklam · cała historia · CPM 10–25 zł · 3 marki naraz”). W środku:
   **Pobieranie marek** — domyślnie 3 marki naraz (szybciej o ok. 40–50% przy kilku konkurentach); przy błędach limitu zapytań zmień na 2 albo „po kolei”.
   Opcjonalnie ustaw **limit reklam na konto** — domyślny dla wszystkich i osobno dla każdej marki (np. 250 dla dużego konkurenta). Przy limicie bot bierze najpierw aktywne reklamy, potem zakończone. Wyniki takiej marki są oznaczone jako **próba** (żółty baner), bo liczby bezwzględne — liczba reklam, zasięg, koszt, udział w głosie — są wtedy zaniżone; wskaźniki procentowe są bardziej miarodajne. Przy limicie zakładka „Zmiany” nie zapisuje migawki.
   Pod spodem wybierz **okres** („Z jakiego okresu”): cała historia, ostatnie 7 / 14 dni, ostatni miesiąc, 3 miesiące, pół roku, rok, poprzedni miesiąc, ten / poprzedni kwartał, ten / poprzedni rok albo **własny zakres dat**. Okres jest wspólny dla wszystkich kont, żeby porównanie było uczciwe. Liczą się reklamy **emitowane choć jeden dzień w okresie** (także uruchomione wcześniej); limit reklam liczy się już tylko z nich. Zasięg i wydatki z API dotyczą całej emisji reklamy, nie samego okresu — bot pokazuje to w banerze z ikoną kalendarza, w PDF, Excelu („Zakres danych”) i pliku dla AI. Przy wybranym okresie zakładka „Zmiany” nie zapisuje migawki.
   **Tendencje i porównania** (pole pod okresem) — jak liczyć zmiany w czasie, gdy wybrany jest okres:
   - **porównaj z poprzednim analogicznym okresem** (domyślnie) — bot pobiera dodatkowo okres tuż przed wybranym (np. 3 miesiące przed ostatnimi 3 miesiącami; dla „poprzedni miesiąc/kwartał/rok” — poprzedni kalendarzowy; dla „ten rok” — ten sam fragment roku wcześniej). Tendencja = okres vs poprzedni, a w Przeglądzie pojawia się tabela „Porównanie okresów” (nowe i emitowane reklamy, koncepty, szac. wyświetlenia, koszt, zasięg, oferty, rabat, wideo — z % zmiany), w Porównaniu marek: „Zmiana względem poprzedniego okresu” dla każdej marki;
   - **porównaj na tle całej historii** — pobiera całą dostępną historię do końca okresu (najwolniej): pełne wykresy 12 miesięcy plus to samo porównanie okres do okresu;
   - **tylko wybrany okres** — nic więcej nie pobiera. Porównania wymagające danych sprzed okresu są oznaczone **„b.d.” (brak danych)**, a nie liczone jako zero; tendencja to druga połowa okresu vs pierwsza (dla okresów od 28 dni), a wykresy miesięczne obejmują tylko miesiące z danymi (niepełny miesiąc oznaczony *). W kalendarzu dni bez danych mają przerywaną ramkę.
   Wartości „w okresie” (wyświetlenia, koszt, zasięg) to część zasięgu reklamy przypadająca na dni okresu. **Dane starsze niż ok. 12 miesięcy:** Meta usuwa reklamy komercyjne z biblioteki ok. roku po zakończeniu emisji, więc starszy okres jest w danych zaniżony i porównanie z nim dawałoby sztuczne wzrosty (np. „+1500%”). Dlatego gdy poprzedni okres sięga dalej niż ok. 12 miesięcy wstecz (np. „ten rok” vs ten sam fragment zeszłego roku, „ostatni rok” vs rok wcześniej): bot **nie pokazuje procentowej zmiany** (w tabelach „—”, poprzedni okres oznaczony jako „niepełny”), tendencję liczy jako **ostatnie 90 dni vs poprzednie 90 dni** (oba okna w pełni dostępne), nie używa takiego porównania we wnioskach i rekomendacjach, a w Excelu i pliku dla AI dopisuje uwagę. Podpowiedź przy polu „Tendencje i porównania” uprzedza o tym już przy wyborze okresu. Jeśli nawet okna 90-dniowe są starsze niż rok (np. „poprzedni rok”), tendencja = „brak wiarygodnego porównania”.
7. Tam też język reklam, próg **evergreen** (ile dni emisji traktujemy jako „sprawdzoną” reklamę, domyślnie 30) oraz założenia kosztu (benchmarki, CPM, częstotliwość).
8. Kliknij **Analizuj**. Pod przyciskiem pojawi się panel postępu: każda marka z aktualnym krokiem (szukanie strony, pobieranie reklam, formaty, reklamy społeczne), liczbą pobranych reklam, czasem i paskiem postępu.

Jeśli bot trafił na złą stronę (np. fanpage zamiast oficjalnej), w zakładce „Przegląd” tej marki są przyciski z innymi znalezionymi stronami — kliknij właściwą.

---

### Zapis i wczytanie analizy
Po analizie przycisk **„Zapisz analizę”** zapisuje pobrane dane i ustawienia do pliku `.json`. **„Wczytaj analizę”** otwiera go później — bez tokenu i bez pobierania (dobre do prezentacji i jako archiwum). Wczytana analiza ma baner „stan danych na …”, a wszystkie wskaźniki liczone są na dzień pobrania. Kliknięcie „Analizuj” wraca do aktualnych danych.

### Założenia kosztu dla poszczególnych marek (CPM, częstotliwość, CPM wg formatu)
„Ustawienia zaawansowane” → „Założenia dla poszczególnych marek”. Lista ma **jeden wiersz na każdą markę** z formularza — Twoją markę i każdego konkurenta (licznik nad listą pokazuje np. „Pola dla 6 marek: Twoja marka + 5 konkurentów”). Wiersze dopisują się i znikają same, gdy zmieniasz listę konkurentów, dodajesz ich z „Odkryj konkurentów po frazie” albo wczytujesz zestaw; ta sama marka wpisana dwa razy to jeden wiersz, a wpisane wartości są przypisane do marki (nie do pozycji na liście), więc zmiana kolejności konkurentów ich nie przestawia. Dla każdej marki możesz wpisać:
- **CPM marki** (min–max) i **częstotliwość marki** (min–max),
- **CPM wg formatu kreacji** (przycisk „CPM wg formatu”): wideo, grafika, grafika z tekstem (mem), inne (karuzela, katalog, tekst).

Jak bot dobiera CPM dla każdej reklamy: **1.** CPM formatu tej marki → **2.** CPM marki → **3.** założenia globalne (benchmark albo kalibracja). Puste pole dziedziczy wartość z poziomu wyżej — widać ją jako podpowiedź w polu. Pole uzupełnione częściowo albo błędnie (np. min > max, częstotliwość < 1) jest pomijane i oznaczone na czerwono — bot niczego nie zgaduje.

Koszt marki liczony jest dla każdej reklamy z jej stawką; w wynikach widać to jako **efektywny CPM** (średnia stawek ważona udziałem formatów w zasięgu). W Przeglądzie pod szacunkiem kosztu i w zakładce „Prawdopodobny budżet” jest tabela **„Skąd ten CPM”**: format, udział w zasięgu, użyta stawka i jej źródło. Marka z własnymi założeniami ma w nagłówku znacznik „własne założenia kosztu”. CPM wg formatu działa tylko, gdy włączone jest „Pobieraj typ mediów” — inaczej bot wyraźnie pisze, że go nie użył. Założenia marek zapisują się w przeglądarce, w zestawach i w zapisanej analizie; trafiają też do Excela (arkusz „Założenia kosztu”), PDF i pliku dla AI.

### Kalibracja na realnym wydatku
W „Ustawieniach zaawansowanych” → „Kalibracja na realnym wydatku”: wybierz markę, której prawdziwy wydatek znasz (np. klienta agencji z Menedżera Reklam), wpisz kwotę i okno (ostatnie 30 dni albo wybrany okres) i kliknij „Skalibruj CPM”. Bot wylicza efektywny CPM = wydatek ÷ (szacowane wyświetlenia ÷ 1000) i ustawia CPM ±15% wokół niego dla wszystkich marek. Ręczna zmiana CPM albo wybór benchmarku wyłącza kalibrację.

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

Na górze wybierasz **Porównanie** albo konkretną markę (liczba obok nazwy to indeks aktywności 0–100). Niżej — zakładki tematyczne. Każda zaczyna się od wyróżnionej ramki (ikona żarówki) z najważniejszym wnioskiem.

### Zakładki marki

| Zakładka | Co zawiera |
| --- | --- |
| **Przegląd** | KPI (w tym szacowane wyświetlenia), indeks aktywności, najważniejsze w skrócie, trend 12 miesięcy, szacowane wyświetlenia, szacowany koszt, największe aktywne reklamy |
| **Prawdopodobny budżet** | Przedziały wydatków z prawdopodobieństwem (99% / 95% / 90% / 80% / 50%), rozkład, kalkulator „jakie są szanse, że wydaje więcej niż X zł”, co najbardziej wpływa na niepewność. Własne CPM, częstotliwość i inne założenia — działają tylko w tej zakładce |
| **Kreacje i testy** | % reklam wyłączanych w ≤7 dni, reklamy działające równolegle, DCO, formaty, platformy, długość życia reklam, efektywność zasięgu, evergreeny |
| **Kampanie** | Koncepty kreatywne (ten sam tekst = jeden koncept) i fale startów (dni z ≥3 nowymi reklamami) |
| **Przekaz** | Ton (sprzedażowy / wizerunkowy), CTA, emoji, długość tekstów, najczęstsze słowa i hasła, mechaniki, strony docelowe, nagłówki |
| **Oferty i ceny** | Rabaty (średni, maksymalny, rozkład), ceny, mechaniki promocyjne, reklamy z najwyższym rabatem. Rabat liczy się tylko w jednoznacznym kontekście („-30%”, „rabat 20%”, „15% taniej”) — statystyki („87% kobiet”), składy („50% mniej cukru”) i zakresy nie są promocją; progi dostawy, bony, nagrody i kwoty rabatów nie są cenami |
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
| 🟪 **Dedukcja detektywa** | Interpretacje narzędzia | ramki z żarówką, „Najważniejsze w skrócie”, rekomendacje |

Przed wysłaniem raportu klientowi warto włączyć „Poszlaki” i „Dedukcję”, żeby świadomie zdecydować, co zostawić.

---

## 6. Eksporty: PDF, Excel, plik dla AI, pakiet do strategii

### Pakiet do strategii (PNG + dane)
Przycisk **„Pakiet do strategii (PNG + dane)”** po analizie. Wybierasz wykresy porównania, wykresy marek i dla których marek, motyw (jasny do prezentacji / ciemny), rozdzielczość (1×/2×/3×) i podpis. Dostajesz jeden plik `.zip`:
- `wykresy/` — każdy wykres jako PNG z tytułem, nazwą marki i podpisem źródła (Meta Ad Library API · rodzaj danych · data), uporządkowane w folderach: porównanie + osobny folder na markę, plus obraz „Kluczowe wskaźniki” każdej marki;
- `dane/fakty-do-strategii.csv` — tabela marka · obszar · wskaźnik · wartość · jednostka · pochodzenie (dane / obliczenia / szacunek) · uwagi — gotowa do wklejenia do strategii;
- `dane/raport_meta_….xlsx` — pełny Excel (wszystkie arkusze + „Fakty do strategii”), same wartości, bez formuł;
- `CZYTAJ-MNIE.md` — zakres analizy, jak czytać liczby, założenia szacunków i lista plików z opisem.

Pojedynczy wykres pobierzesz też od razu z wyników: najedź na wykres i kliknij **PNG** przy jego tytule. PDF z danymi to przycisk „Raport PDF”.

### Raport PDF

Przycisk **Raport PDF** → panel z ustawieniami:

- tytuł, klient, autor, logo na okładkę, komentarz analityka,
- wybór sekcji w trzech grupach (Ogólne / Porównanie / Strony każdej marki) i marek,
- gotowe zestawy: **Pełny raport** (~50 stron przy 4 markach) i **Skrót dla klienta** (~10 stron).

Otworzy się okno drukowania — jako drukarkę wybierz **„Zapisz jako PDF”**. Jeśli tła się nie drukują, zaznacz „Grafika tła”.

### Excel

Przycisk **Pobierz Excel** — 18 zakładek (porównanie, podsumowanie, rekomendacje, tendencja, kreacje i testy, kampanie, kalendarz, evergreeny, copy, przekaz, rabaty i ceny, demografia, luki, targetowanie, zmiany, reklamy społeczne, wszystkie reklamy). **Same wartości, bez formuł.**

### Eksport dla AI (.md)

Plik Markdown z gotowym promptem i danymi — do wklejenia lub załączenia w Claude, ChatGPT lub innym AI.

1. Wybierz, co AI ma przeanalizować (pozycja konkurencyjna, kreacje, komunikacja, oferty, odbiorcy, budżet, kalendarz, zmiany, reklamy społeczne, rekomendacje, brief kreatywny) albo gotowy zestaw.
2. Wybierz formę odpowiedzi: **Audyt** (analiza → hipotezy → problemy → plan), **raport dla klienta**, **notatka dla zespołu** albo **konspekt prezentacji**.
3. Uzupełnij branżę, cel marketingowy i kontekst — im więcej, tym trafniejsza analiza.
4. Ustaw liczbę tekstów reklam w pliku (więcej = lepsza analiza komunikacji, ale większy plik).
5. **Pobierz plik .md** albo **Kopiuj do schowka**. „Podgląd” pokazuje, co dokładnie trafi do AI.

Prompt każe AI opierać się wyłącznie na danych z pliku, odróżniać dane od szacunków (te same znaczniki co Lupa detektywa) i oznaczać hipotezy. Opcja „Dołącz wnioski bota” dodaje automatyczne rekomendacje do zweryfikowania przez AI.

---

## 7. Zestawy konkurentów i migawki

**Zestawy** (przyciski u góry formularza): zapisz markę, konkurentów, obszar, język, limity reklam i okres pod nazwą, np. „Natura — drogerie”, i wczytuj jednym kliknięciem. „Eksport / Import” przenosi zestawy między komputerami.

**Migawki** zapisują się automatycznie po każdej analizie (jedna dziennie na stronę i obszar). Przy kolejnej analizie — najwcześniej następnego dnia — zakładka **Zmiany** pokaże różnice. Migawki żyją w przeglądarce: przed zmianą komputera lub czyszczeniem danych zrób „Eksport migawek”.

---

## 8. Słowniczek wskaźników

| Wskaźnik | Definicja |
| --- | --- |
| **Aktywna reklama** | Bez daty zakończenia emisji albo z datą od dziś wzwyż |
| **Evergreen** | Reklama emitowana co najmniej X dni (ustawiasz sam, domyślnie 30) |
| **Zasięg** | `eu_total_reach` — szacunek Meta: ile kont w UE zobaczyło reklamę |
| **Szacowany koszt** | Zasięg dzienny aktywnych reklam × częstotliwość × CPM (z benchmarku albo kalibracji). Widełki = krańce założeń, „najbardziej prawdopodobnie” = mediana modelu — w Przeglądzie i w zakładce „Prawdopodobny budżet” to ta sama liczba. Aktywne reklamy bez danych o zasięgu (Meta podaje go tylko dla emisji w UE) są doszacowane proporcjonalnie, a pokrycie jest podane pod szacunkiem |
| **Szacowane wyświetlenia** | Zasięg × częstotliwość. Meta nie podaje wyświetleń reklam komercyjnych, tylko zasięg — to szacunek. Pokazywane dziennie, miesięcznie, za ostatnie 30 dni i łącznie dla wszystkich reklam |
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
| **Wolne pobieranie dużej marki** | Ustaw limit reklam dla tego konta (np. 500) albo wyłącz „Pobieraj demografię…” i „Pobieraj typ mediów”. |

---

## 11. Prywatność i bezpieczeństwo

- Plik działa w całości w Twojej przeglądarce. Dane idą tylko między przeglądarką a Meta (graph.facebook.com). Biblioteka do Excela ładuje się z cdnjs, czcionka z Google Fonts.
- Token zapisuje się tylko lokalnie i tylko wtedy, gdy zaznaczysz „Zapamiętaj token”. **Nie wysyłaj tokena nikomu** — działa jak hasło.
- Zestawy, migawki, ustawienia i logo do PDF są w pamięci przeglądarki (localStorage) na tym komputerze.
