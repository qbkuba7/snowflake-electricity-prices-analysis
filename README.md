### Projekt: snowflake-electricity-prices-analysis
Analiza cen prądu oraz sensu wprowadzania magazynów energii przy użyciu technologii Snowflake, Python - (streamlit, pandas, numpy), SQL.
Głównym zadaniem projektu było stworzenie projektu interaktywnej aplikacji analitycznej przeznaczonej zgodnie z następującymi User Stories:
* US1: Jako osoba zainteresowana przejściem na taryfy dynamiczne chcę sprawdzić, ile zapłaciłbym miesięcznie za prąd (przy zadanym zużyciu miesięcznym (np. 200kWh)) uwzględniając historyczne ceny energii w TGE. 
* US2: Jako osoba zainteresowana przejściem na taryfy dynamiczne chcę sprawdzić, jak zamontowanie magazynu energii wpłynie na moje rachunki za prąd. Magazyn ma swoje parametry: pojemność (1-20kWh), maksymalny prąd ładowania (1-11kW), efektywność baterii (90%), stały pobór prądu (15W). 
* US3: Jako sprzedawca magazynów energii chcę móc policzyć klientowi optymalny rozmiar magazynu, jaki w oparciu o dane historyczne pozwoli mu zminimalizować koszty energii.

# Technologie:
* **Platforma:** Snowflake (Notebooks, Streamlit apps)
* **Język:** SQL, Python (pandas, numpy, streamlit, altair)


# Dane:

Fundamentem aplikacji są dane rynkowe i ustandaryzowane profile konsumenckie zużycia prądu:
1. **Historyczne ceny prądu:** Godzinowe ceny prądu pobrane ze strony: (https://energy.instrat.pl/ceny/energia-rdn-godzinowe/).
2. **Zużycie energii:** Udostępnione profile standardowe zużycia energii elektrycznej ze strony: (https://www.stoen.pl/)

Dane zostały pobrane do Snowflake i odpowiednio sformatowane w celu łatwiejszego zarządzania.
W ostatecznym podejściu każda godzina jest oceniana w kontekście lokalnego okna czasowego (24 godziny w przód i w tył). Na podstawie obliczonych kwantyli (10%, 25%, 75%) dla tego okna, algorytm przypisuje cenie w danej godzinie etykietę: `"najtaniej"`, `"tanio"`, `"średnio"` lub `"drogo"` i na tej podstawie system wie, kiedy optymalnie ładować a kiedy korzystać ze zgromadzonej w magazynie energii.

Poniżej znajduje się podgląd przetworzonej tabeli na podstawie której działa cała logika aplikacji (wywołanie `df.head(30)`):

![Podgląd przetworzonych danych](./projsnow1.png)

# Prezentacja aplikacji i realizacja User Stories

Poniżej przedstawiono działanie interaktywnego dashboardu w odniesieniu do zdefiniowanych celów projektowych:
1. Analiza kosztów przy taryfach dynamicznych (Realizacja US1)

Interaktywne filtry dat oraz suwak miesięcznego zużycia pozwalają użytkownikowi natychmiastowo przeliczyć historyczny koszt energii. Dashboard dynamicznie agreguje dane (dziennie, tygodniowo, miesięcznie), prezentując średnią cenę [zł/MWh] oraz łączny szacowany koszt.

![Podgląd przetworzonych danych](./projsnow2.png)
2. Symulacja wpływu magazynu na rachunki (Realizacja US2)

Zaimplementowany algorytm symuluje pracę magazynu w oparciu o techniczne parametry wejściowe. System podejmuje decyzje o ładowaniu ("najtaniej/tanio") i rozładowywaniu ("drogo/średnio"), uwzględniając przy tym straty energii (efektywność) oraz stały pobór własny urządzenia.

![Podgląd przetworzonych danych](./projsnow3.png)
3. Optymalizacja rozmiaru magazynu dla klienta (Realizacja US3)

Dzięki czytelnym wykresom oszczędności (net saving) oraz porównaniu "Cost with battery" vs "Estimated cost", sprzedawca lub inwestor może przetestować różne pojemności magazynu (np. 5kWh vs 10kWh), aby znaleźć punkt optymalny, w którym oszczędności najlepiej kompensują koszt inwestycji.

![Podgląd przetworzonych danych](./projsnow4.png)
