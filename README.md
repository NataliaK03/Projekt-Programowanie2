# Wykrywanie oszustw kart kredytowych – Credit Card Fraud Detection

## Opis danych

Dane pochodzą z projektu badawczego Université Libre de Bruxelles (ULB).
Zawierają transakcje kart kredytowych wykonane przez europejskich posiadaczy kart we wrześniu 2013.

| Właściwość | Wartość |
|---|---|
| Liczba rekordów | 284 807 |
| Liczba cech (po selekcji) | 15 |
| Typ zadania | Klasyfikacja binarna |
| Źródło | Kaggle – mlg-ulb/creditcardfraud |

| Cecha | Opis |
|---|---|
| V1–V28 | Cechy po transformacji PCA (anonimizacja danych wrażliwych) |
| Amount | Kwota transakcji |
| Time | Czas od pierwszej transakcji w zbiorze (sekund) – odrzucona |
| **Class** | **Zmienna celu: 0 = legalna, 1 = oszustwo (fraud)** |

Rozkład klas: ~99.83% legalnych, ~0.17% fraudów (silne niezbalansowanie).

## Cel analizy

Zbudowanie i porównanie modeli uczenia maszynowego do wykrywania oszustw finansowych.
Ze względu na silne niezbalansowanie klas, kluczową metryką jest **F1-score dla klasy Fraud (1)**.

## Metodologia

1. **Pobieranie danych:** Kaggle API (`opendatasets`)
2. **Eksploracja danych (EDA):** rozkład klas, rozkłady kwot i czasu, boxploty
3. **Analiza korelacji:** korelacja cech z Class, macierz korelacji top 10 cech
4. **Czyszczenie danych:** brak braków, usunięcie duplikatów
5. **Selekcja cech:** top 14 cech wg korelacji z Class + Amount → 15 cech wejściowych
6. **Próbkowanie:** stratyfikowana próbka 80k rekordów do modelowania
7. **Podział:** 80% trening / 20% test (stratyfikowany)
8. **Standaryzacja:** StandardScaler
9. **Obsługa niezbalansowania:** `class_weight='balanced'` we wszystkich modelach
10. **Modele:**
    - Regresja logistyczna (GridSearchCV: C, solver)
    - Drzewo decyzyjne (GridSearchCV: max_depth, min_samples_split, criterion)
    - Random Forest (RandomizedSearchCV: n_estimators, max_depth, min_samples_split, max_features)
11. **Ewaluacja:** classification_report, F1-score, macierze błędów, ważność cech

## Wyniki

| Model | F1-score (Fraud) | Recall (Fraud) |
|---|---|---|
| Regresja logistyczna | ~0.80 | ~0.90 |
| Drzewo decyzyjne | ~0.78 | ~0.82 |
| **Random Forest** | **~0.86** | **~0.85** |

*Dokładne wartości zależą od wyników RandomizedSearchCV – patrz notebook.*

## Najlepszy model: Random Forest

Random Forest osiągnął najwyższy F1-score dla klasy Fraud.
Najważniejsze predyktory: V14, V10, V12, V17, Amount.

## Uruchomienie

Notebook działa w Google Colab. Dane pobierane przez Kaggle API (`opendatasets`).

**Wymagane:** konto Kaggle + API token (`kaggle.json`) – szczegóły w notebooku (komórka 1).

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

## Technologie

Python 3 | pandas | numpy | scikit-learn | matplotlib | seaborn | opendatasets
