# Podstawy Analizy Danych
# Superstore

Projekt zaliczeniowy z przedmiotu Podstawy Analizy Danych. Analiza eksploracyjna zbioru transakcji sprzedażowych Superstore wraz z testami statystycznymi i weryfikacją hipotezy badawczej.

## Dane

Zbiór Superstore (sprzedaż detaliczna, USA): 9994 transakcje z lat 2014-2017, 21 kolumn, m.in. daty zamówienia i wysyłki, segment klienta, lokalizacja, kategoria asortymentu, wartość sprzedaży, ilość, rabat i zysk. Plik źródłowy: `data/superstore.xls`.

## Zawartość repozytorium

| Plik | Opis |
|---|---|
| `eda_superstore.ipynb` | główny notebook z całą analizą |
| `data/superstore.xls` | dane źródłowe |
| `README.md` | ten plik |

## Przebieg analizy (etapy notebooka)

1. **Wczytanie i wstępny przegląd** - struktura danych, statystyki opisowe, decyzje o doborze kolumn (m.in. redukcja hierarchii geograficznej do poziomu Region).
2. **Czyszczenie danych** - analiza braków (NaN i puste stringi), usunięcie duplikatów, naprawa wiodących zer w kodach pocztowych, nowa cecha Handling Time (czas obsługi zamówienia), usunięcie zbędnych kolumn.
3. **Eksploracyjna analiza danych** - histogramy, wykrywanie outlierów (reguła IQR), sezonowość liczby zamówień (dekompozycja szeregu na trend, sezon i resztę; indeks sezonowy dla wolumenu, sprzedaży i zysku).
4. **Analiza** - skośność i kurtoza zmiennych numerycznych, statystyki zmiennych kategorycznych, testy Manna-Whitneya (porównania parami z korektą Bonferroniego i wielkością efektu rank_biserial), testy chi-kwadrat z weryfikacją założeń (oczekiwane liczebności, niezależność obserwacji).
5. **Analiza korelacji** - macierz korelacji rang Spearmana.
6. **ANOVA i ANCOVA** - weryfikacja założeń (Levene, D'Agostino-Pearson), ANOVA Welcha, post-hoc Tukey HSD; ANCOVA świadomie nieprzeprowadzona po stwierdzeniu złamania założenia równoległości nachyleń (interakcja Category x Sales).
7. **Hipoteza badawcza** - czy rabaty obniżają zysk transakcji: jednostronny test Manna-Whitneya oraz korelacja Spearmana.
8. **Wnioski końcowe**.

## Najważniejsze wnioski

- Zbiór jest kompletny (brak braków danych); zawiera liczne wartości odstające w Sales i Profit - są informatywne, więc pozostały w analizie, a dobór metod przesunięto w stronę technik rangowych i odpornych.
- Kategorie asortymentu różnią się istotnie pod względem Sales, Profit i Discount; nie różnią się pod względem Quantity ani Handling Time.
- Mix segmentów, regionów i sposobów wysyłki jest niezależny od kategorii produktu (chi-kwadrat, wszystkie p > 0,7).
- Średni zysk różni się między kategoriami (ANOVA Welcha, p < 0,001); najbardziej dochodową kategorią jest Technology.
- Wpływ sprzedaży na zysk silnie zależy od kategorii (złamana równoległość nachyleń): z 1 dolara sprzedaży najwięcej zysku zostaje w Office Supplies, najmniej w Furniture - dlatego klasycznej ANCOVA nie przeprowadzono.
- Liczba zamówień ma wyraźny cykl roczny (szczyty we wrześniu, listopadzie i grudniu) nałożony na rosnący trend rok do roku.
- Hipoteza potwierdzona: transakcje z rabatem mają istotnie niższy zysk niż transakcje bez rabatu (Mann-Whitney, p < 0,001; Spearman rho = -0,54). Rabaty obniżają rentowność.

## Środowisko i uruchomienie

Python 3.13 oraz biblioteki: pandas, numpy, matplotlib, seaborn, scipy, statsmodels, pingouin, skimpy, xlrd.

```
pip install pandas numpy matplotlib seaborn scipy statsmodels pingouin skimpy xlrd jupyter
jupyter notebook eda_superstore.ipynb
```

Notebook wczytuje dane ścieżką względną `data/superstore.xls`, dlatego należy uruchamiać go z katalogu głównego repozytorium.
