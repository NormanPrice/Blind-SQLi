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
       W tym zadaniu na pewno będziesz potrzebował Burp Intruder.
    </li>
  </ol>
</details>

<details>
  <summary>Druga podpowiedź</summary>
  <ol>
    <li>
      Na początek trzeba będzie zbadać długość hasła (użyj funkcji  LENGTH()), 
    </li>
    <li>
      Znając długość hasła można badać kolejne litery hasła (użyj funkcji SUBSTRING()).
    </li>
  </ol>
</details>
<br/>


## Blind SQL injection with conditional errors
### Zadanie 2.1 
Aplikacja używa śledzenia ciasteczek do analizy i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka. 
Wyniki zapytania SQL nie są zwracane, aplikacja nie reaguje w żaden inny sposób w zależności od tego, czy zapytanie zwróci cokolwiek. Jeśli zapytanie SQL spowoduje błąd, wówczas aplikacja zwraca niestandardowy komunikat błędu.
Baza danych zawiera tabelę o nazwie **users**, z kolumnami o nazwach **username** i **password**. 
Aby rozwiązać zadanie, zaloguj się jako użytkownik **administrator**.
Długość hasła jest identyczna co w poprzednim zadaniu.
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
<br/>
<br/>

## Blind SQL injection with time delays
### Zadanie 3.1
Aplikacja używa śledzenia ciasteczek do analizy i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka. 
Jednak wyniki zapytania SQL nie są zwracane, a także aplikacja zachowuje się cały czas tak samo- niezależnie od tego co dane zapytanie SQL zwraca. Możliwe jest jednak spowodowanie opóźnienia, dzięki któremu można wnioskować pewne informacje.
Zadanie polega na wykorzystaniu podatności SQLi w celu spowodowania 10 sekundowego opóźnienia.
<br/>
<br/>

- [Zadanie](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays)
<details>
  <summary>Pierwsza podpowiedź</summary>
  <ol>
    <li>
       W tym zadaniu na pewno będziesz potrzebował Burp Intruder
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

<br/>

### Zadanie 3.2 
<br/>
Aplikacja używa śledzenia ciasteczek do analizy i wykonuje zapytanie SQL zawierające wartość przesłanego ciasteczka. 
Jednak wyniki zapytania SQL nie są zwracane, a także aplikacja zachowuje się cały czas tak samo- niezależnie od tego co dane zapytanie SQL zwraca. Możliwe jest jednak spowodowanie opóźnienia, dzięki któremu można wnioskować pewne informacje.
Baza danych zawiera tabelę o nazwie <b>users</b>, z kolumnami o nazwach <b>username</b> i <b>password</b>. 
<br/>
W tym zadaniu musisz się zalogować na konto  <b>administrator</b>. Dla ułatwienia zadania jego hasło składa się z takiej samej ilości znaków co poprzednie hasła.
<br/>
<br/>

- [Zadanie](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval)
- [Lista znaków występujących w haśle](https://github.com/NormanPrice/Blind-SQLi/blob/main/litery)
<details>
  <summary>Pierwsza podpowiedź</summary>
  <ol>
    <li>
       W tym zadaniu na pewno będziesz potrzebował Burp Intruder, i należy dopisać zapytanie SQL w tym samym miejscu co w poprzednim zadaniu 
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


<br/>


