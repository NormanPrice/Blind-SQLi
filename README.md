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
### Lab 1.1 - Username enumeration przez różne odpowiedzi
Zadanie - Skutecznie stwierdzić, które konto użytkownika istnieje i złamać jego hasło, a następnie wejść na podstronę konta.
To laboratorium jest podatne na `username enumeration` i atak `brute-force` na hasła.
Zarówno nazwa użytkownika jak i hasło znajdują się na poniższych listach:

Witryna daje *zdecydowanie za dużo informacji* użytkownikom i atakującemu.
- [Zadanie](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval)
- [Podpowiedzi](https://github.com/LittleBigKiller/bawim-auth/blob/master/Lab1-Hints.md#lab-11---username-enumeration-przez-r%C3%B3%C5%BCne-odpowiedzi)

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

