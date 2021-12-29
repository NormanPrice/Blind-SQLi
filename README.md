# Blind-SQLi
Poniższe zadania mają na celu przedstawić temat podatnosći Blind SQL injection

<br/>

Przed rozpoczęciem pracy z laboratoriami należy zarejestrować się lub zalogować w serwisie [Portswigger](https://portswigger.net/).
Przydatne mogą okazać się również [payload'y](https://portswigger.net/web-security/sql-injection/cheat-sheet).
A także należy zainstalować [Burp Suite](https://portswigger.net/burp/communitydownload). 
<br/><br/>

---
## Blind SQL injection with conditional responses
### Zadanie 1.1
Aplikacja wykorzystuje ciasteczko śledzące do celów analitycznych i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka.
Wyniki zapytania SQL nie są zwracane, nie są też wyświetlane żadne komunikaty o błędach. Aplikacja zawiera jednak komunikat <b>"Welcome back"</b> na stronie, jeśli zapytanie zwróci jakiekolwiek wiersze.
Baza danych zawiera tabelę o nazwie **users**, z kolumnami o nazwach **username** i **password**. 
Aby rozwiązać zadanie, zaloguj się jako użytkownik **administrator**.
<br/>
<br/>

- [Zadanie](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses)
- [Lista znaków występujących w haśle](https://github.com/NormanPrice/Blind-SQLi/blob/main/litery)
<details>
  <summary>Pierwsza podpowiedź</summary>
  <ol>
    <li>
       W tym zadaniu napewno będziesz potrzebował Burp Intruder.
    </li>
  </ol>
</details>

<details>
  <summary>Druga podpowiedź</summary>
  <ol>
    <li>
      Na początek trzeba będzie zbadać długość hasła(użyj funkcji  LENGTH()), 
    </li>
    <li>
      Znając długość hasła można badać badać kolejne litery hasła(użyj funkcji SUBSTRING()).
    </li>
  </ol>
</details>

<details>
  <summary>Krok po kroku</summary>
  <ol>
    <li> Z włączonym w tle Burpem wejdź na stronę sklepu  </li>
    <li> Znajdź w żądaniu taką linijkę „Cookie: TrackingId=jakaś_zawartość; session=jakaś_zawrtość” </li>
    <li> Zmodyfikuj  Cookie: TrackingId=jakaś_zawartość<b>' AND '1'='1</b>; session=jakaś_zawrtość” - sprawdź czy występuje jakiś komunikat </li>
    <li>Zmodyfiikuj jedną "1" na dowolny inny znak - sprawdź czy strona reaguje prawidłowa</li>
    <li>Wyślij zapytanie, nad którym pracujesz, do Burp Intrudera</li>
    <li>W zakładce Positions programu Burp Intruder wyczyść domyślne pozycje klikając na przycisk "Clear §".</li>
    <li> Zmodyfikuj  Cookie: TrackingId=jakaś_zawartość<b>' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a</b>; session=jakaś_zawrtość” </li>
    <li>Umieść znacznik "Add §"  wokół znaku '1' w wartości cookie. </li>
    <li>Przejdź do zakładki Payloads w polu Payload type wybierz Numbers</li>
    <li>Poniżej w polu Payload Options wybierz zakres od 1 do 30 i krok 1</li>
    <li>Następnie przejdź do zakładki Options i w polu Grep-Match naciśnij Clear, potem wpisz    <b>Welcome back</b> i kliknij Add </li>
    <li>Rozpocznij atak - długość hasła to liczba przy której nie pojawi się już komunikat <b>Welcome back</b></li>
    <li>Przejdź ponownie do zakładki Positions i zastąp wcześniej dodane zapytanie sql następującym <b>' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a</b></li>
    <li>Umieść znacznik "Add §"  wokół znaku 'a' w wartości cookie.</li>
    <li>Przejdź do zakładki Payloads w polu Payload type wybierz Simple list</li>
    <li>Poniżej w polu Payload Options dodaj plik do którego link umieszczony został w tym zadniu</li>
    <li>Jeśli wszytko się zgadza rozpocznij atak</li>
    <li>Znak przy którym się pojawi komunikat<b>Welcome back</b> jest pierwszą literą hasła</li>
    <li>Przejdź ponownie do zakładki Positions i w funkcji <b>SUBSTRING(password,1,1)</b> zamień pierwszą 1 na 2: <b>SUBSTRING(password,2,1)</b> </li>
    <li>Rozpocznij ponownie atak </li>
    <li>Znak przy którym się pojawi komunikat <b>Welcome back</b> jest drugąliterą hasła</li>
    <li>Postępuj analogicznie inkrementując do warości będącej długością hasła</li>
    <li>Jeśli masz już całe hasło zaloguj się na konto administratora używając loginu <b>administrator</b> i hasła które już posiadasz</li>
  </ol>
</details>
<br/>


## Blind SQL injection with conditional errors
### Zadanie 2.1 
Aplikacja używa śledzenia ciasteczek do analizy i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka. 
Wyniki zapytania SQL nie są zwracane, aplikacja nie reaguje w żaden inny sposób w zależności od tego, czy zapytanie zwróci cokolwiek. Jeśli zapytanie SQL spowoduje błąd, wówczas aplikacja zwraca niestandardowy komunikat błędu.
Baza danych zawiera tabelę o nazwie **users**, z kolumnami o nazwach **username** i **password**. 
Aby rozwiązać zadanie, zaloguj się jako użytkownik **administrator**.
Długość hasła jest identyczna co w poprzednim zadaniu
<br/>
<br/>

- [Zadanie](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors)
- [Lista znaków występujących w haśle](https://github.com/NormanPrice/Blind-SQLi/blob/main/litery)
<details>
  <summary>Pierwsza podpowiedź</summary>
  <ol>
    <li>
       W tym zadaniu także będziesz potrzebował Burp Intruder, oraz wystarczy zmienić treść zapytania SQL 
    </li>
  </ol>
</details>

<details>
  <summary>Druga podpowiedź</summary>
  <ol>
    <li>
     Najpierw ustal z jakiej bazy danych korzysta aplikacja, a następnie skorzystaj z zapytania które pojawiło się w prezentacji.
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

## Blind SQL injection with time delays
### Zadanie 3.1
Aplikacja używa śledzenia ciasteczek do analizy i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka. 
Jednak wyniki zapytania SQL nie są zwracane, a także aplikacja zachowuje się cały czas tak samo- niezależnie od tego co dane zapytanie SQL zwraca. Możliwe jest jednak spowodowanie opóźnienia, dzięki któremu można wnioskować pewne informacje.
Zadanie polega na wykorzystaniu podatnosci SQLi w celu spowodowania 10 sekundowego opóźnienia.
<br/>
<br/>

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

### Zadanie 3.2 
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
       W tym zadaniu napewno będziesz potrzebował Burp Intruder, i należy dopisać zapytanie sql w tym samym miesjcu co w poprzednim zadaniu 
    </li>
  </ol>
</details>

<details>
  <summary>Druga podpowiedź</summary>
  <ol>
    <li>
    To zapytanie może być przydatne ' %3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTRING(password,(numer litery),1)='a')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users-- 
    </li>
  </ol>
</details>

<details>
  <summary>Krok po kroku</summary>
  <ol>
    <li> Z włączonym w tle Burpem wejdź na stronę sklepu  </li>
    <li> Znajdź w żądaniu taką linijkę „Cookie: TrackingId=jakaś_zawartość; session=jakaś_zawrtość” </li>
    <li> Najpierw sprawdźmy czy użytkownik "administrator" istnieje wklejając poniższą linijkę w tą samo mijesce co w poprzednim zadaniu
      <br/>
    ' %3BSELECT+CASE+WHEN+(username='administrator')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--</li>
    <li>Jeśli storna odpowiada po dłuższym czasie (10s) znaczy to że użytkownik istnieje </li>
    <li>Wyślij zapytanie, nad którym pracujesz, do Burp Intrudera</li>
    <li>W zakładce Positions programu Burp Intruder wyczyść domyślne pozycje klikając na przycisk "Clear §".</li>
    <li>Zmień wartość cookie na: TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTRING(password,1,1)='a')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
    </br>
      Wykorzystuje to funkcję SUBSTRING() do wyodrębnienia pojedynczego znaku z hasła i przetestowania go względem określonej wartości. Nasz atak będzie cyklicznie przechodził przez każdą pozycję i możliwą wartość, testując każdą z nich po kolei.
    <li>Umieść znacznik "Add §"  wokół znaku 'a' w wartości cookie.</li>
    <li>Przejdź do zakładki Payloads, sprawdź czy wybrana jest opcja "Simple list", a następnie w zakładce "Payload Options" dodaj znaki z pliku do którego link znajduję się w tym zadaniu</li>
    <li>Aby móc stwierdzić, kiedy właściwy znak został wysłany, będziesz musiał monitorować czas potrzebny aplikacji na odpowiedź na każde żądanie. Aby proces ten był jak najbardziej niezawodny, musisz skonfigurować atak Intrudera tak, aby wysyłał żądania w pojedynczym wątku. Aby to zrobić, przejdź do zakładki Resource Pool i dodaj atak do puli zasobów (na dole strony) z ustawionym parametrem "Maximum concurrent requests" na 1.</li>
    <li>Rozpocznij atak obserwując wyraźnie zauważalne opóźnienie w przypadku jedego znaku - zanotuj go </li>
    <li>Po zakończeniu pierwszej rundy przejdź do zakładni Positions i zmień argument funkcji "Subsrting" z 1 na 2 SUBSTRING(password,<b>2</b>,1)</li>
    <li>Postępuj anologicznie inkrementując wartość do 20</li>
  <li>Zaloguj się na konto administratora używając loginu <b>administrator</b> i hasła które już znasz</li>
  </ol>
</details>
<br/>


## Blind SQL injection with out-of-band interaction
### Zadanie 4.1
Aplikacja używa śledzenia ciasteczek do analizy i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka. 
Zapytanie SQL jest wykonywane asynchronicznie i nie wpływa na odpowiedź aplikacji. Jednak da się spowodować interakcję z zewnętrzną domeną.
Zadanie polega na wykorzystaniu podatności SQLi w celu spodowania DNS lookup dla burpcollaborator.net.

- [Zadanie](https://portswigger.net/web-security/sql-injection/blind/lab-out-of-band)

<br/>

