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
<details>
  <summary>Drobna podpowiedź</summary>
  <ol>
    <li>
      Burp Intruder będzie bardzo przydatny w tym laboratorium, darmowa wersja Burpa w zupełności wystarczy.
    </li>
  </ol>
</details>

<details>
  <summary>Duża podpowiedź</summary>
  <ol>
    <li>
      Kluczem w tym laboratorium jest przyjżenie się uważnie odpowiedziom na zapytanie <code>POST /login</code>. 
    </li>
    <li>
      Próbuj łamać hasło dopiero jak poznasz poprawną nazwę użytkownika.
    </li>
  </ol>
</details>

<details>
  <summary>Krok po kroku</summary>
  <ol>
    <li> Z włączonym w tle Burpem wejdź na stronę logowania i wyślij żądanie logowania z całkowicie losowymi danymi. </li>
    <li> W Burpie wejdź w zakładkę "Proxy" > "HTTP history" i znajdź zapytanie <code>POST /login</code>. Wyślij je do Burp Intruder. </li>
    <li> W Burp Intruder, wejdź w zakładkę "Positions". Upewnij się, że <b>attack type</b> ma ustawioną wartość "Sniper". </li>
    <li>
      Naciśnij przycisk "Clear", żeby wyczyścić wszelkie automatycznie przypisane pozycje payloadu. Zaznacz wartość parametru <code>username</code>
      i kliknij "Add", żeby ustawić ją jako pozycję payloadu. Pozycja ta zostanie oznaczona przez dwa symbole <code>§</code>, na przykład:
      <code>username=§invalid-username§</code>. Na razie pozostaw hasło jako dowolną wartość stałą.
    </li>
    <li> W zakładce "Payloads", upewnij się, że <b>payload type</b> jest ustawiony na wartość "Simple List" </li>
    <li>
      W sekcji "Payload Options", wklej <a href=https://portswigger.net/web-security/authentication/auth-lab-usernames>listę użytkowników</a>.
      Nareszcie możemy rozpocząć atak! W tym celu wciśnij klawisz "Start attack". Atak rozpocznie się w nowym oknie.
    </li>
    <li>
      Gdy atak zakończy się, w zakładce "Results" zbadaj kolumnę "Length". Możesz kliknąć w nagłówek kolumny, aby posortować zawarte w niej dane.
      Zauważ, że jedna z wartości jest dłuższa od pozostałych. Porównaj zawartość tej odpowiedzi z pozostałymi. Zwróć uwagę, że pozostałe odpowiedzi
      zawierają wiadomość <code>Invalid username</code>, a ta zawiera <code>Incorrect password</code>. Zanotuj nazwę użytkownika w kolumnie "Payload".
    </li>
    <li>
      Zamknij okno ataku i wróć to zakładki "Positions". Kliknij "Clear", a następnie zmień wartość parametru <code>username</code> na wartość,
      którą udało się zdobyć w poprzednim kroku. Dodaj pozycję payloadu jako wartość parametru <code>password</code>. Rezultat powienien wyglądać
      mniej więcej w ten sposób:<br/><code>username=identified-user&password=§invalid-password§</code>
    </li>
    <li>
      W zakładce "Payloads", wyczyść listę nazw użytkowników i zastąp ją <a href=https://portswigger.net/web-security/authentication/auth-lab-passwords>
      listą potencjalnych haseł</a>. Kliknij "Start attack".
    </li>
    <li>
      Gdy atak zakończy się, popatrz na kolumnę "Status". Zauważ, że każdy request otrzymał odpowiedź z kodem statusu 200, poza jednym,
      który dostał odpowiedź z kodem 302. To sugeruje, że dana próba logowania była skuteczna.
    </li>
    <li> Zaloguj się przy użyciu zidentyfikowanych nazwy użytkownika i hasła i wejdź na podstronę konta użytkownika w celu rozwiązania laboratorium. </li>
  </ol>
</details>

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

