---
title: Po co testy jednostkowe?
category: paradygmaty-programowania
thumbnail: testowanie-oprogramowania.png
published: 2021-04-21T00:00:00Z
---
Wielu programistów ma bardzo **różne podejście do testów** jednostkowych. Niektórzy piszą, bo muszą. Inni nie lubią lub nie rozumieją, trzymają się z daleka. **Co tak naprawdę dają nam testy jednostkowe** i czy warto zaprzątać sobie nimi głowę?

<!--more-->

## Czym są testy jednostkowe?

Testy jednostkowe to metoda testowania wytwarzanego oprogramowania, polegająca na pisaniu metod testujących określone, małe fragmenty naszego programu (jednostki). Jednostkami mogą być np. metody lub klasy.
{: .alert-info} 

Zaczynając przygodę z programowaniem ciężko dostrzec **zalety pisania testów jednostkowych**. Wszystko wydaje się niby proste, jednak nasuwa się pytanie: **po co testować coś, co sami piszemy?**

W tym artykule przytoczę kilka najważniejszych faktów i zalet pisania testów jednostkowych.

## Testy jednostkowe są dla programistów

Jako programista **powinieneś uwielbiać testy jednostkowe** - one są stworzone dla Ciebie. Pilnują Twojego kodu! Dzięki testom jednostkowym przestaniesz popełniać błache błędy, ponieważ istnieje dodatkowa warstwa kontroli. Ta warstwa będzie grać na Twoją korzyść im większy będzie projekt.

Jeżeli programista nie chce pisać testów jednostkowych **to prawdopodobnie po prostu nie będzie**. Nikt go do tego nie zmusi, a jeżeli zmusi, to testy będą tak słabej jakości, że będą niezbyt użyteczne.

Dlaczego testy jednostkowe są dla programistów? Ponieważ wyłapują one błędy zlokalizowane "u źródła", w najniższej możliwej warstwie. Im więcej testów jednostkowych tym mniej błędów odkrytych na produkcji i tłumaczenia się klientom.

## Test jednostkowe wymagają poprawnego pisania kodu

Jeżeli jesteś programistą, który nie lubi pisać testów jednostkowych to przepraszam, ale **prawdopodobnie tworzysz kod słabej jakości**. Prawdziwą zaletą testów jednostkowych jest to, że **nie tolerują one żadnej prowizorki**.

Funkcje, które testujemy muszą być małe. Muszą odpowiadać za pojedyncze czynności, mieć jak najmniej parametrów i najlepiej robić jak najmniej rzeczy - czyli wszystko to, o czym mówi "czysty kod". Jeżeli masz skłonności do tworzenia super klas i wrzucasz wszystko do jednego worka, to nijak nie uda Ci się napisać dobrych testów.

Dla mnie nakaz pisania testów jednostkowym w projekcie jest dobry własnie z tego powodu. Programiści zaczynają tworzyć solidny kod. Zaczynają używać wzorców projektowych, odpowiednio wydzielają funkcje, oddzielają logikę aplikacji od logiki biznesowej.

## Testy jednostkowe we wzorcu AAA

Zatrzymajmy się chwilę przy kodzie dobrej jakości. Moim zdaniem najlepsza metodą na pisanie testów jednostkowych jest trzymanie się wzorca AAA (ang. *Arrange, Act and Assert*). Oznacza to po prostu: ustaw, uruchom, sprawdź! Jeżeli jesteś w stanie pisać testy używając AAA, to Twój kod jest nieskazitelnie idealny i chciałbym z Tobą pracować.

Oto przykład takiego testu:

    void ShouldGenerateCorrectUserLogin() {
        // arrange
        const name = "John";
        const surename = "Green";
        const company = "Umbrella";
        
        // act
        const result = service.generateUserLogin(name, surename);
        
        // assert
        expect(result).toEqual("john.green@umbrella.com");
    }

Jak widzisz w powyższym przykładzie, testujemy trywialną funkcję. Sprawdzamy czy złącza stringa w odpowiedni sposób, czy wszystkie litery są małe - niby nic wielkiego.

Bardzo zachęcam Cię do spróbowania pisania testów w ten sposób. Zwróć szczególną uwagę na trzy literki AAA: deklarujesz zmienne, uruchamiasz, sprawdzasz rezultat.

Jeżeli testując wyjdzie Ci coś takiego, to wiedz, że dzieje się coś złego:

    void ShouldDoSomething() {
        // arrange
        const something1 = "John";
        const orderStrategy = strategies.get(something1);
        const something2 = "Green";

        // act
        const order = service.foo(something2, orderStrategy);

        // assert
        expect(order).toBeDefined();

        // act
        const result2 = service.buz(order.status);
        
        // assert
        expect(result2).toEqual(true);
    }

Wiem, że wzorzec AAA wydaje się prosty, jednak gdy zaczniesz pisać pierwsze testy, napotkasz duże trudności. Nie zniechęcaj się! Z czasem jakość Twojego kodu mocno wzrośnie.

## Nie ma refaktoryzacji bez testów

Jako programista lub jako osoba związana z IT musisz zrozumieć jedną ważną zasadę: **nie ma refaktoryzacji bez testów**. Duży fragment newralgicznego kodu, który nie został pokryty testami, naddaje się do zalania betonem aby pozostał w swojej niezmiennej postaci już na zawsze.

Nie ma możliwości refaktoryzowania wrażliwych fragmentów systemu nie posiadając żadnej warstwy, która zweryfikuje nasze zmiany. W takim wypadku najpierw trzeba pokryć kod testami, a dopiero później przeprowadzać refaktoryzację. Oczywiście, jeżeli w kodzie nie było ani jednego testu, to **prawdopodobnie kod jest całkowicie nietestowalny**. Zataczamy wtedy błędne koło.

Oto programistyczny deadlock, sytuacja bez wyjścia, której nigdy Ci nie życzę:

{% include parts/postPicture.html page=page img="testy-jednostkowe" %}

Oczywiście, ktoś może uprzeć się na refaktoryzację - czy testy są czy nie. Jednak wtedy jest to jazda bez trzymania i efekty, których bliżej nie da się przewidzieć. Garść błędów zostanie wychwycona podczas developmentu, trochę na środowisku testowym, a pewna ilość na produkcji. W zalezności od systemu, który rozwijamy, takie rozwiązanie może być nie do zaakceptowania.

## Testowanie front-endu

Programiści języków silnie typowanych jak C++, Java, C# mogą spać spokojnie, ponieważ kompilator niezwykle skutecznie wyłapuje wszelkie błędy już na etapie kompilacji. Programiści JavaScript mają zadanie bardzo utrudnione. W JS wszystko jest dynamiczne i **jest to język, który niezwykle mocno potrzebuje testów jednostkowych**.

Sytuację na rynku bardzo odmienił TypeScript, jednak nie wszędzie jest to cudowane rozwiązanie. Po pierwsze nie wszytkie projekty używają TypeScripta, a po drugie nie wszędzie da się wszystko pokryć typami (naprawdę!).

Pracowałem niedawno w projekcie, gdzie front-end mapował żądanie API różnie w zależności od typu bazy. Jeden serwis zapisywał dane w bazie relacyjnej, drugi w bazie obiektowej. Sam obiekt był bardzo skomplikowany, jego atrybuty były różne i miały bardzo dynamiczne nazwy. Dla mnie były one logiczne (powiązanie z miesiącami i latami) jednak używając TypeScripta nie dało się pokryć tego modelu typami. Ratunkiem były oczywiście testy jednostkowe, które sprawdzały czy mapowanie odbywa sę w poprawny sposób.

Osobiście uważam, że w czasach mocno rozwiniętego front-endu testy bardziej potrzebne są właśnie na froncie. Testy jednostkowe warto mieć wszędzie, jednak front-end bez testów jest o wiele trudniejszy do ogarnięcia.

## Duże projekty i wielu programistów

Kto kiedyś dołączył do dużego i starego projektu, ten umie docenić testy jednostkowe. Duży projekt pisany latami niekiedy może przypominać spagetti. Każdy programista dopisuje do projektu swoje własne metody i klasy, rozwija istniejące funkcje i dodaje nowe. W takim przypadku **brak testów może w przyszłości nieść wiele negatywnych skutków**.

Pisanie kodu w dużej mierze polega na używaniu istniejących funkcji. Nierzadko stare funkcje i mechanizmy trzeba dostosować do nowych wymagań. Jako nowi programiści w dużym projekcie nigdy nie mamy pewności co można zmienić, a co nie. W takich przypadkach testy jednostkowe bardzo często ratują nas przed problemami.

## Szkoda czasu na testy? Oj nie..

Możesz pomyśleć, że **szkoda Ci czasu na pisanie testów**, jednak cały ten **czas zwróci się z nawiązką**. Zadanie, które możesz napisać w 5h z testami, napiszesz bez testów w 3h. Pozornie zaoszczędziłeś 2h pracy, jednak napisałeś nietestowalnego bubla. 

Teraz każdy inny programista, który będzie musiał w jakiś sposób skorzystać z Twoich metod, klas, modułów, **będzie musiał przeprowadzać testy ręczne**. Będzie musiał linijka po linijce starać się zrozumieć, dlaczego w 97 linijce jest jakaś dziwna instrukcja warunkowa, sprawdzająca czy zmienna _priceDiff_ jest równa zero, i dlaczego jak jest równa zero to wyskakujesz z obiektu pętli. Jeżeli po jakimś czasie da radę się domyślić - pół biedy. Jeżeli nie da rady się domyślić a projekt skompiluje się bez błędów, to ktoś i tak zmarnuje czas szukając później błędów w działaniu produkcji.

## Jakie ma być pokrycie kodu testami?

Pisanie testów wydłuża proces tworzenia oprogramowania. M.in. z tego powodu **nierealne jest aby projekt był mocno pokryty testami**. Jest to niepotrzebne, wręcz może prowadzić do błędów. To ile testów napisać zależy od czasu jakim dysponujemy i przede wszystkim od przeczucia. Warto testować fragmenty kodu, które są **bardzo newralgiczne i używane w wielu miejscach**.

Dobre pokrycie testami to 40%, jednak czy ta informacje coś mówi? Można mieć duże pokrycie testami a żadnych korzyści, lub małe pokrycie testami i wielkie korzyści. Zależy to od jakości testów i elementów, które testujemy.

## Podsumowanie

W każdym z wypadków wiele czasu da się oszczędzić **świadomie pisząc testy jednostkowe** dla odpowiednich elementów systemu. Nie chodzi tu o to, aby starać się być z testami w granicach 30-40% pokrycia. To tylko wskaźnik, który może mówić wszystko albo nic. W sztuce pisania testów **należy wiedzieć co testować i gdzie testy będą niezbędne**, a gdzie okażą się stratą czasu.

**Czy pisanie testów wydłuża proces pisania oprogramowania?** - zdecydowanie tak, dlatego testy trzeba pisać mądrze, pokrywać testami tylko wrażliwe elementy systemu.

**Po co testować coś co sami napisaliśmy?** Nie piszemy testu, aby przetestować własny kod. Testy piszemy na zapas, to jak inwestycja na przyszłość. Test nie ma na celu wychwycenia naszych błędów, tylko ewentualne błędy podczas przyszłej refaktoryzacji (szczególnie jeżeli wykona ją ktoś inny niż my). Jeżeli tworzymy jakąś funkcję np. kalkulator walut, zakładamy że jesteśmy jedyną osobą, która ma wiedzieć jak ma ten kalkulator działać. Inni nie wiedzą jak to zrobić.