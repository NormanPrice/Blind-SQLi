# Blind-SQLi
Poniższe zadania mają na celu przedstawić temat podatnosći Blind SQL injection

<br/>

Przed rozpoczęciem pracy z laboratoriami należy zarejestrować się i zalogować w serwisie [Portswigger](https://portswigger.net/).
Przydatne mogą okazać się również [payload'y](https://portswigger.net/web-security/sql-injection/cheat-sheet).
<br/><br/>

---

## Zadanie 1 - Blind SQL injection with time delays
Aplikacja używa śledzenia ciasteczek do analizy i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka. 
Jednak wyniki zapytania SQL nie są zwracane, a także aplikacja zachowuje się cały czas tak samo- niezależnie od tego co dane zapytanie SQL zwraca. Możliwe jest jednak spowodowanie opóźnienia, dzięki któremu można wnioskować pewne informacje.
Zadanie polega na wykorzystaniu podatnosci SQLi w celu spowodowania 10 sekundowego opóźnienia.

- [Zadanie](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays)

<br/>

## Zadanie 2 - Blind SQL injection with out-of-band interaction
Aplikacja używa śledzenia ciasteczek do analizy i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka. 
Zapytanie SQL jest wykonywane asynchronicznie i nie wpływa na odpowiedź aplikacji. Jednak da się spowodować interakcję z zewnętrzną domeną.
Zadanie polega na wykorzystaniu podatności SQLi w celu spodowania DNS lookup dla burpcollaborator.net.

- [Zadanie](https://portswigger.net/web-security/sql-injection/blind/lab-out-of-band)

<br/>

## Zadanie 3 - Blind SQL injection with conditional responses
Aplikacja używa śledzenia ciasteczek do analizy i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka. 
Rezultat zapytania nie jest zwracany, komunikaty błędu również nie są wyświetlane. Aplikacja zawiera jednak wiadomość "Welcome back", jeżeli zapytanie zwraca jakiś wiersz tabeli.
Baza danych zawiera tabelę *users*, z kolumnami *nazwa użytkownika* oraz *hasło*. Zadanie polega na ustaleniu hasła do administratora.

- [Zadanie](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses)

