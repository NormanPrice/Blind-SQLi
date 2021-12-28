# Blind-SQLi
Poniższe zadania mają na celu przedstawić temat podatnosći Blind SQL injection

<br/>

Przed rozpoczęciem pracy z laboratoriami należy zarejestrować się i zalogować w serwisie [Portswigger](https://portswigger.net/).
Przydatne mogą okazać się również [payload'y](https://portswigger.net/web-security/sql-injection/cheat-sheet).
<br/><br/>

---

## Blind SQL injection with time delays
### Zadanie 1.1
Aplikacja używa śledzenia ciasteczek do analizy i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka. 
Jednak wyniki zapytania SQL nie są zwracane, a także aplikacja zachowuje się cały czas tak samo- niezależnie od tego co dane zapytanie SQL zwraca. Możliwe jest jednak spowodowanie opóźnienia, dzięki któremu można wnioskować pewne informacje.
Zadanie polega na wykorzystaniu podatnosci SQLi w celu spowodowania 10 sekundowego opóźnienia.

- [Zadanie](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays)
<details>
  <summary>Pierwsza podpowiedź</summary>
  <ol>
    <li>
       W tym zadaniu napewno będziesz potrzebował Burp Intruder
    </li>
  </ol>
</details>

<details>
  <summary>Druga podpowiedź</summary>
  <ol>
    <li>
     W którymś miejscu żądania trzeba będzie dopisać pg_sleep(czas opóźnienia) 
    </li>
  </ol>
</details>

<details>
  <summary>Krok po kroku</summary>
  <ol>
    <li> Z włączonym w tle Burpem wejdź na stronę sklepu  </li>
    <li> Znajdź w żądaniu taką linijkę „Cookie: TrackingId=jakaś_zawartość; session=jakaś_zawrtość” </li>
    <li> Zmodyfikuj  Cookie: TrackingId=jakaś_zawartość<b>’ ||pg_sleep(10)--</b>; session=jakaś_zawrtość” </li>
    <li> Wyślij żądanie i poczekaj 10 s </li>
  </ol>
</details>
<br/>

### Zadanie 1.2 
<br/>
Aplikacja używa śledzenia ciasteczek do analizy i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka. 
Jednak wyniki zapytania SQL nie są zwracane, a także aplikacja zachowuje się cały czas tak samo- niezależnie od tego co dane zapytanie SQL zwraca. Możliwe jest jednak spowodowanie opóźnienia, dzięki któremu można wnioskować pewne informacje.
Baza danych zawiera tabelę o nazwie <b>users</b>, z kolumnami o nazwach <b>username</b> i <b>password</b>. 
<br/>
W tym zadaniu musisz się zalogować na konto  <b>administrator</b>. Dla ułatwienia zadania jego hasło zawiera 20 znaków
<br/>
<br/>

- [Zadanie](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval)
- [Lista znaków występujących w haśle](https://github.com/NormanPrice/Blind-SQLi/blob/main/litery)
<details>
  <summary>Pierwsza podpowiedź</summary>
  <ol>
    <li>
       W tym zadaniu napewno będziesz potrzebował Burp Intruder
    </li>
  </ol>
</details>

<details>
  <summary>Druga podpowiedź</summary>
  <ol>
    <li>
     W którymś miejscu żądania trzeba będzie dopisać pg_sleep(czas opóźnienia) 
    </li>
  </ol>
</details>

<details>
  <summary>Krok po kroku</summary>
  <ol>
    <li> Z włączonym w tle Burpem wejdź na stronę sklepu  </li>
    <li> Znajdź w żądaniu taką linijkę „Cookie: TrackingId=jakaś_zawartość; session=jakaś_zawrtość” </li>
    <li> Zmodyfikuj  Cookie: TrackingId=jakaś_zawartość<b>’ ||pg_sleep(10)--</b>; session=jakaś_zawrtość” </li>
    <li> Wyślij żądanie i poczekaj 10 s </li>
  </ol>
</details>
<br/>

<br/>

## Zadanie 2 - Blind SQL injection with conditional responses
Aplikacja używa śledzenia ciasteczek do analizy i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka. 
Rezultat zapytania nie jest zwracany, komunikaty błędu również nie są wyświetlane. Aplikacja zawiera jednak wiadomość "Welcome back", jeżeli zapytanie zwraca jakiś wiersz tabeli.
Baza danych zawiera tabelę *users*, z kolumnami *nazwa użytkownika* oraz *hasło*. Zadanie polega na ustaleniu hasła do administratora.

- [Zadanie](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses)
<br/>

## Zadanie 3 - Blind SQL injection with out-of-band interaction
Aplikacja używa śledzenia ciasteczek do analizy i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka. 
Zapytanie SQL jest wykonywane asynchronicznie i nie wpływa na odpowiedź aplikacji. Jednak da się spowodować interakcję z zewnętrzną domeną.
Zadanie polega na wykorzystaniu podatności SQLi w celu spodowania DNS lookup dla burpcollaborator.net.

- [Zadanie](https://portswigger.net/web-security/sql-injection/blind/lab-out-of-band)

<br/>

