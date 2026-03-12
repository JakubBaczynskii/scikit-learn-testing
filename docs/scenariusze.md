# Scenariusze Testów Akceptacyjnych – scikit-learn

Poniższy dokument definiuje zbiór testów akceptacyjnych dla modułu `scikit-learn`. Testy te weryfikują działanie biblioteki z perspektywy użytkownika końcowego (Out Of The Box) w najpopularniejszych przypadkach użycia.

---

## Scenariusz 1: Rozpoznawanie nowotworów (Zdolność do klasyfikacji)

- **Cel testu:** Sprawdzenie gotowości modelu klasyfikacyjnego (np. `RandomForestClassifier`) do poprawnego uczenia się na rzeczywistym zbiorze danych oraz generowania użytecznych przewidywań.
- **Oczekiwany rezultat:** Po wczytaniu wbudowanego zbioru danych `breast_cancer` i podzieleniu go na zbiór treningowy oraz testowy, model pomyślnie przejdzie proces trenowania (metoda `.fit()`) bez rzucania wyjątków, a następnie wygeneruje tablicę predykcji (metoda `.predict()`).
- **Kryterium zaliczenia:** Wynik dokładności (metryka `accuracy_score`) modelu obliczony na zbiorze testowym wynosi co najmniej 85%.

---

## Scenariusz 2: Budowa i walidacja potoku przetwarzania (Pipeline)

- **Cel testu:** Weryfikacja prawidłowego działania klasy `Pipeline`, która jest kluczowa w zapobieganiu wyciekom danych (data leakage) podczas transformacji i uczenia.
- **Oczekiwany rezultat:** Zbudowany potok, składający się z transformatora wstępnego (np. `StandardScaler` do normalizacji danych) oraz estymatora końcowego (np. `SVC` - Support Vector Classification), poprawnie przetworzy surowe dane wejściowe. Transformator przeskaluje dane wewnętrznie i przekaże je bezpośrednio do modelu, ukrywając złożoność przed użytkownikiem.
- **Kryterium zaliczenia:** Wywołanie `pipeline.predict()` na nowych, nieskalowanych wcześniej danych testowych nie kończy się błędem i zwraca tablicę przewidywań o długości odpowiadającej liczbie próbek wejściowych.

---

## Scenariusz 3: Zapisywanie i wczytywanie modelu (Persystencja)

- **Cel testu:** Upewnienie się, że wyuczony model uczenia maszynowego może zostać bezpiecznie zapisany na dysk i odzyskany w celu ponownego użycia, co jest fundamentem wdrażania modeli na produkcję (tzw. deployment).
- **Oczekiwany rezultat:** Przetrenowany model zostanie wyeksportowany do pliku (np. przy użyciu natywnej w ekosystemie biblioteki `joblib`), usunięty z pamięci programu, a następnie załadowany ponownie do nowej zmiennej. Odtworzony model zachowa wszystkie wyuczone wagi i strukturę.
- **Kryterium zaliczenia:** Odtworzony z dysku model generuje w 100% identyczne prognozy (wektor wyników) dla tego samego zestawu danych testowych, co oryginalny model przed jego zapisaniem.