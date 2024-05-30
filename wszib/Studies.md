
## Things to do:
####  Java #1 project due 21.01.2024
<details>
<summary>Spring project</summary>
 Na projekt zaliczeniowy z przedmiotu Spring składa się aplikacja.
Aplikacja musi mieć przynajmniej dwa profile - jeden który używa bazy danych (np.
dockerowej, może być to MySQL, może być Postgres, nawet może być H2, tutaj jaka
zostanie użyta baza danych to zostawiam Państwu do wyboru), drugi który będzie używał
“bazy danych” in memory może nawet nie tyle co baza danych co przechowywanie danych w
listach (wzorzec DAO).
Aplikacja musi mieć przynajmniej 6 endpointów:
- GET: /api/property?name={propertyName} - do pobierania zmiennych
(propertisów) aplikacji
- GET - endpoint do pobierania danych z bazy danych, zarówno całej listy jak i
pojedyńczej danej za pomocą id przez path variable
- POST - do tworzenia danych w bazie danych
 - PATCH - do aktualizowania danych w bazie danych
 -DELETE - do usuwania danych w bazie danych
Ponad to muszą być użytkownicy z rolami: ADMIN, USER.
ADMIN - może wszystko.
USER - może tylko wyświetlać dane w endpointach typu GET.
Niezalogowany użytkownik nie może zobaczyć i zmodyfikować żadnych danych.
Ponad to proszę o pamiętaniu o takich rzeczach jak:
- przynajmniej dwa testy jednostkowe - jeden który testuje poprawne zachowanie,
drugi niepoprawne czyli np. wyrzucanie wyjątków bądź podanie niepoprawnych
danych
- przynajmniej dwa testy integracyjne - j. w. - jeden na poprawne zachowanie, drugi na
niepoprawne
- walidacji danych przy tworzeniu nowych rekordów, np. id nie można podać lub jakaś
nazwa jest wymagana to musi zostać podana a nie, że zostanie utworzony pusty
rekord
Uwaga:
Mogą zostać wykorzystane szablony html, może zostać wykorzystany zwykły html ale też
projekt może być w formie wystawienia api i dane zwracane jak i podawane w postaci JSON.
PS: Dodatkowy punkt jeśli państwo napiszą czym się endpoint PUT od PATCH bez patrzenia
w zasoby internetu :)
</details>
- 

#### Java #2 project due 21.01.2024
#### Biznesplan dokończenie działu i wysłanie due 10.01.2024
#### Teoria gier komputerowych, oddac gre due 21.01.2024
#### Zarządzanie projektem informatycznym- prepare presentation due 21.01.2024
#### Prawo komputerowe - exam due (?)
#### Przygotować papiery na praktyki - zanieść do podpisania
#### Inżynierka
- Backend
- Frontend
- Teoria

