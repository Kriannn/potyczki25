# Witamy w finale!
Potyczki Młodych Adminów 2025

## Wstęp

Tegoroczne zadania będą testem nie tylko znajomości Kubernetes i Ranchera, ale też działania pod presją, ze szczególnym uwzględnieniem zagadnień bezpieczeństwa.

Tematem konkursu jest praca w jednej z "trzyliterowych służb" - tak tajnych, że nawet nie wymawiamy głośno tych liter. Dostaniemy trochę typowo administracyjnych zadaniań, bo przecież tajne systemy też wymagają obsługi i utrzymania - ale też zmierzymy się z działaniami agentów Rosyjskich, których aktywność będzie wymagała podjęcia odpowiednich kroków!

Powodzenia!

---
### Misja specjalna

Specjalna operacja woj... a nie, czekaj, nie mamy na to kilku lat - MISJA specjalna zostanie ogłoszona o czasie T+15minut. Obecność obowiązkowa. Misja może odmienić losy kraju - a nawet tego konkursu!

Proszę o przygotowanie sobie następujących linków (na razie pozostają prywatne, zostaną upublicznione po ogłoszeniu):

https://github.com/jbiniek/potyczki25-doomwar

https://github.com/jbiniek/potyczki25-moonlanding

### Misja 0 - kryptonim "Red Tape"
Najlepiej napisany program i najlepiej wdrożony system jest tykającą bombą bez odpowiedniej dokumentacji. Nikt nie lubi jej pisać, ale jest kluczowa dla łatwości późniejszego utrzymywania i efektywnej współpracy - a także do sprawdzania zadań! Dlatego udokumentuj wszystkie zadania, co najmniej oznaczając te które zostały wykonane, bo **tylko te zostaną sprawdzone**. Niektóre zadania wymagają pisemnej odpowiedzi, umieść je też w dokumentacji. Zalecany - nie, JEDYNIE SŁUSZNY - sposób na przedstawienie dokumentacji to utworzenie repozytorium GitHub (polecam utworzenie forka tego repo).

Opisz kroki wykonane w celu realizacji zadania, szczególnie lokalizacje zasobów, użyte opcje i komendy - nie musisz tego robić bardzo dokładnie, ale w razie wątpliwości będą one działać na twoją korzyść. Na przykład jeśli zadanie nie zostało do końca wykonane, ale znacząca część kroków jest opisana poprawnie, zaliczę za to częściowe punkty. Albo jeśli zadanie zostało wykonane, ale nie w sposób jakiego oczekiwałem, to opis będzie kluczem do uzyskania za nie punktów. To, co nie jest opisane, a nie jest oczywiste z interfejsu Ranchera, będzie rozstrzygane na twoją niekorzyść!

### Misja 1 - operacja "Otwarte Okno"
Potrzebujemy nowego serwera webowego do ogłaszania krytycznych informacji ze Sztabu Zarządzania Kryzysowego, ale minister cały dzień jadł ośmiorniczki i dlatego dopiero teraz dotarły rozkazy. Wszyscy inni poszli już do domu, więc cała nadzieja w waszym zespole.
Na klastrze "potyczki" utwórz projekt "szk-server" a w nim namespace "ogloszenia-krytyczne". W tym namespace uruchom serwer webowy. Nie mówią jaki, więc użyj dowolnego, ale ma być w najnowszej wersji. **3pkt**
- Utwórz usługę dzieki której można się odwołać do naszego serwera z całego klastra **3pkt**
- Instrukcje mówią o wysokiej dostępności tej usługi - nie masz dostępu do większej liczby maszyn, więc zrób co się da, żeby zwiększyć jej dostępność w obrębie istniejących zasobów **3pkt**
- Skonfiguruj serwer aby serwował załączony plik index.html **4pkt**
- Zapewnij dostępność usługi na internet. Nie masz czasu czekać do jutra aż administratorzy sieci udostępnią ci firmowy DNS, a potrzebujesz szybko przetestować dostępność, więc wymyśl jak zapewnić rozwiązywalny url wskazujący na IP hosta, na którym jest twój klaster "potyczki". **18pkt** (pełnym sukcesem operacji jest podanie adresu typu ogloszenia-krytyczne.xxxx.xxx rozwiązywalnego przy pomocy publicznego DNS z internetu, pod którym zgłosi się działająca strona internetowa);

Rozwiazanie:
Tworzymy projekt i namespace:

![Zrzut ekranu 2025-05-16 113440](https://github.com/user-attachments/assets/d593cee9-b5fa-4e49-8897-8ef119f56b0d)

Tworzymy config mapa by go podmontowac do podow:

![Zrzut ekranu 2025-05-16 114413](https://github.com/user-attachments/assets/7bcff25b-1367-40f0-b0a7-6b4f8a7730d6)

Tworzymy deloyment i dajemy podom labele asd asd by matchowac z serwisow:
![Zrzut ekranu 2025-05-16 123306](https://github.com/user-attachments/assets/51b1063a-ce08-4bc9-8767-be609228ccb0)


Udostepniamy deployment jako NodePort:

![Zrzut ekranu 2025-05-16 115037](https://github.com/user-attachments/assets/dfe6a825-c7d5-48fd-8142-3965b31fd98c)
![Zrzut ekranu 2025-05-16 115050](https://github.com/user-attachments/assets/18f56888-1869-46b4-b24b-ec3ae9969faf)

![Zrzut ekranu 2025-05-16 115836](https://github.com/user-attachments/assets/8a2fa199-460f-4eb6-81dd-054431db5f61)
Tworzymny ingresa ktory mapuje wszystko z domeny sslip do serwisu wczesniej utworzonego

![Zrzut ekranu 2025-05-16 120012](https://github.com/user-attachments/assets/551ba20f-7db4-49e3-9c8d-139a2f08f418)
Dzialajaca strona

### Misja 2 - kryptonim "Long Horn"
Potrzebujemy Persistent Storage, szybko! Tylko musi być taki, żeby umożliwiał replikację! Wprawdzie i tak mamy tylko jeden serwer w klastrze, więc musimy ograniczyć liczbę replik do 1, ale sama obsługa replikacji jest ważna dla morale dowództwa - nieważne, że działa tylko na papierze... Nfs provisioner jest wykluczony, potrzebne jest coś lepszego. Gdyby tylko w Rancherze istniało jakieś repozytorium z łatwym w instalacji i obsłudze rozwiązaniem storage dla Kubernetes...

Zainstaluj rozwiązanie typu software-defined storage w najnowszej stabilnej wersji na klastrze "potyczki", ustawiając w konfiguracji instalacyjnej 1 replikę i domyślny StorageClass. **5pkt**
Sukces misji oznacza działającą aplikację storage oraz dostępną StorageClass.

### Misja 3 - operacja "Koci Pazur"
Kot prezesa się nudzi, należy mu zapewnić jakąś rozrywkę.
Dodaj nowe repozytorium do katalogu aplikacji Ranchera. URL repo: https://rancher.github.io/rodeo **5pkt**
![Zrzut ekranu 2025-05-16 114157](https://github.com/user-attachments/assets/710c1990-2714-4119-98ff-d5bf6f58b28c)
![Zrzut ekranu 2025-05-16 114205](https://github.com/user-attachments/assets/f8a5a9ba-cbe8-411e-b387-2e5bde668ca7)

Zainstaluj dowolną grę z nowo dodanego repo. **5 pkt**
![Zrzut ekranu 2025-05-16 123923](https://github.com/user-attachments/assets/24bc8650-7465-4dc2-a6eb-e72381826b19)
![Zrzut ekranu 2025-05-16 123935](https://github.com/user-attachments/assets/644b1a5a-919c-4c48-86ff-44c19f00431b)

### Misja 4 - kryptonim "Armor Plate"
Musimy wzmocnić nasze zabezpieczenia dedykowanymi rozwiązaniami security! Same firewalle to za mało w erze Kubernetes. Potrzebujemy najwyższej klasy ochrony - takiej jaka jest stosowana przez amerykańskie trzyliterowe służby.

Z katalogu aplikacji zainstaluj NeuVector w najnowszej stabilnej wersji. **5pkt**

Włącz funkcję Auto-scan **1 pkt**
![Zrzut ekranu 2025-05-16 114343](https://github.com/user-attachments/assets/f0cca977-f832-4886-9250-1fa4b69e8381)

Zbadaj czy istnieją podatności dla wersji Kubernetes uruchomionej na klastrze potyczki - podaj ich liczbę i jeśli nie jest równa zero, podnieś wersję Kubernetes klastra potyczki do 1.26. **5pkt**
![Zrzut ekranu 2025-05-16 114512](https://github.com/user-attachments/assets/99a27712-c9ab-434e-b740-cb710ed3ec2e)
![Zrzut ekranu 2025-05-16 124402](https://github.com/user-attachments/assets/d282a4a1-2db5-45ce-9e20-f19e882bad8a)

Zbadaj czy istnieją podatności dla węzła (node) klastra potyczki - podaj ich liczbę i jeśli nie jest równa zero, dokonaj patchowania i aktualizacji systemu. **6pkt**
![Zrzut ekranu 2025-05-16 114651](https://github.com/user-attachments/assets/4e007119-7318-4275-9546-2483175f8b3e)

Ciekawe czy wrogie systemy mają podobne podatności, może dałoby się to wykorzystać?...

Rozwiazanie: poczatkowa werjsa klastra 1.24.17+rke2r1 posiada 1 high CVE CVE-2023-5528: Insufficient input sanitization in in-tree storage plugin leads to privilege escalation on Windows nodes.

### Misja 5 - operacja Czyste Ręce
Wdrożenie AI byłoby niesłychanie użyteczne w naszych zadaniach, idealnie byłoby zacząć od narzędzia Ollama. Ale trzeba  się upewnić, że te obrazy nie zawierają podatności - najpierw musimy je przeskanować! 
Użyj NeuVector, żeby przeskanować repozytorium Ollama z rejestru https://registry.hub.docker.com ; jako rozwiązanie podaj nazwę image z największą ilością podatności, oraz liczbę tych podatności. **7pkt**
![Zrzut ekranu 2025-05-16 114900](https://github.com/user-attachments/assets/60f8f884-f64f-4cc8-a168-11367ae3705e)

Rozwiazanie: ollama/quantize:gguf 813 high 1687 medium

### Misja 6 - kryptonim Zero Zaufania
Nasz system wczesnego ostrzegania wykrył podejrzaną aktywność, którą przechwycilismy. Wdróż na klastrze zinfiltrowany zasób w odpowiednim namespace (podejrzany-agent.yaml). Natychmiast odetnij wszelką komunikację sieciową (przychodzącą i wychodzącą) z/do tego poda, żebyśmy mogli go szczegółowo przeanalizować (wymaga pomyślnego ukończenia Misji 4). **10pkt**

Przetestuj działanie zabezpieczeń dla połączeń przychodzących i wychodzących z podejrzanego poda. Załącz do odpowiedzi odpowiednie Security Violations z NeuVectora pokazujące zablokowaną próbę naruszenia blokady.**5pkt**

Utwórz pomocniczego poda nginx o nazwie detektor w namespace kwarantanna. Zmień reguły zabezpieczeń podejrzanego poda, tak, aby dopuszczały połączenie do poda detektor. Wygeneruj ruch sieciowy z podejrzanego-agenta do detektora przy pomocy curl. Użyj NeuVector aby dokonać przechwycenia pakietów z tej komunikacji. **10pkt**

Zablokuj narzędzie curl przy pomocy NeuVector. Potwierdź działanie blokady curl pomimo przepuszczenia ruchu sieciowego do poda detektor (załącz zrzut ekranu lub skopiowane w całości komunikaty shella wraz z poleceniem, które je wyzwoliło). **7pkt**
Wyeksportuj regułę jako CRD w trybie Protect i załącz do dokumentacji (**5 pkt**)

Dokonaj "analizy" przechwyconego pakietu (znajdź odpowiednie narzędzie) - do sukcesu misji wystarczy, że skopiujesz pola opisujące jeden z pakietów: źródłowe i docelowe IP, protokół, długość, info. **7pkt**

### Misja 7 - kryptonim Enigma Reactivation
Dzięki owocnej współpracy z wywiadami innych krajów NATO pomyślnie przechwyciliśmy zaszyfrowaną rosyjską transmisję. Niestety moduł deszyfrujący uległ awarii - po krótkim śledztwie okazało się, że tym razem to nie działanie wroga, ale zwyczajna niekompetecja - ktoś bardzo mądrze użył AI do wygenerowania konfiguracji i zastąpił wszystkie istniejące kopie. Przeanalizuj i napraw deszyfrator-pod-uszkodzony.yaml - bez niego przechwycona transmisja jest bezużyteczna!

Misja zakończona powodzeniem jesli pod deszyfrator przejdzie w stan Completed, a w jego logach pojawi się odszyfrowana wiadomość ("Zneutralizowac agenta KREML. Haslo: BURZA_MAJOWA") - załącz zrzut ekranu lub skopiowane w całości komunikaty shella wraz z poleceniem, które je wyzwoliło. **25pkt**
![Zrzut ekranu 2025-05-16 124830](https://github.com/user-attachments/assets/d1556fc6-a472-4832-90ab-b5f31b857849)
![Zrzut ekranu 2025-05-16 124854](https://github.com/user-attachments/assets/d43a925a-6384-4990-a420-cd7c946895b0)
![Zrzut ekranu 2025-05-16 124957](https://github.com/user-attachments/assets/73b84905-0416-42d2-bffb-dc7db517d0c4)


### Misja 8 - "For Your Eyes Only"
Nowo utworzony zespół szybkiego reagowania natychmiastowo potrzebuje tymczasowego dostępu read-only do logów aplikacji z naszego klastra. Zgodnie z zasadą zero-trust powninniśmy nadać im tylko niezbędne minimum uprawnień.

Utwórz nowego użytkownika rapid-response-agent i nadaj mu dostęp read-only do namespace ogloszenia-krytyczne. **3pkt**

Potrzebujemy dodatkowo kont serwisowych dla zautomatyzowanych narzędzi zespołu szybkiego reagowania. Utwórz specjalną rolę log-reader w namespace ogloszenia-krytyczne, która umożliwia wykonywanie tylko operacji *get pods* oraz *list pods* na podach, oraz - co kluczowe - daje dostęp do zasobu *logs* wewnątrz podów. Utwórz konto serwisowe automated-response-agent w namespace ogloszenia-krytyczne i przypisz mu rolę log-reader. **15pkt**

### Misja 9 - operacja Slinky Slingshot
Rosyjska machina nie ustaje w zalewaniu nas treściami propagandowymi używając wszelkich możliwych kanałów do siania dezinformacji, podgrzewania społecznej niezgody i szczucia na naszych sojuszników. Czas coś z tym zrobić! Masz za zadanie utworzenie Portalu Do Spraw Dez-DezInformacji, w skrócie PDSDDI (prawda, że chwytliwa nazwa?). Portal ma być oparty o wordpress. Utwórz namespace pdsddi i wdróż w nim wordpress.

Nasz portal musi posiadać persistent storage - użyj storage wdrożonego w Misji 2 - chyba, że zakończyła się ona niepowodzeniem, to użyj dowolnej dostępnej alternatywy. **7pkt**

Opublikuj na PDSDDI dwa dowolne fakty na temat Rosji - muszą być prawdziwe, np. "Putin to burak". **2pkt**

Wystaw Portal na świat pod adresem pdsddi.xxxx.xxx, analogicznie jak w Misji 1 **10 pkt**

Opisz podjęte środki w celu zabezpieczenia Portalu (w końcu to tylko wordpress i właśnie wystawiliście go na świat pełen wrogich agentów).**3pkt**

### Misja 10 - "Zaginiona Arka"
Kluczowy system archiwizacji został uszkodzony! Ale to nic dla naszego potężnego storage... Mam nadzieję, że Misja 2 się powiodła bo teraz potrzebujemy jej rezultatów!

Użyj archiwum-delta.yaml, aby utworzyć system archiwizacji. Po uruchomieniu poda, wejdź do jego konsoli i stwórz plik /dane/manifest-delta-v1.txt o treści "Protokół Delta aktywny.". **3pkt** Następnie, w interfejsie Longhorn, utwórz ręcznie backup wolumenu używanego przez tego poda. Teraz zasymuluj uszkodzenie danych poprzez usunięcie pliku manifest-delta-v1.txt z działającego poda. **5pkt**

Znajdź w interfejsie Longhorn ostatni dostępny backup dla wolumenu pvc-archiwum-delta. Następnie przywróć ten backup do *nowego* PersistentVolumeClaim o nazwie pvc-archiwum-delta-przywrocone (w tym samym namespace). Na koniec, przekonfiguruj archiwum-delta, aby używało tego przywróconego wolumenu. Oryginalny, "uszkodzony" PVC (pvc-archiwum-delta) powinien pozostać nietknięty ale odłączony od poda. **6pkt**

Misja doomwar:

Udostepniam jako clusterip bo w wymaganiach jest ze ma byc dostepny dla calego clustra co nie znaczy ze dla calego internetu :)

![Zrzut ekranu 2025-05-16 122739](https://github.com/user-attachments/assets/79cc36af-b643-45da-8a67-175ed838d011)
![Zrzut ekranu 2025-05-16 122823](https://github.com/user-attachments/assets/43ea28d0-ae33-488c-a535-e55e55526d61)
![Zrzut ekranu 2025-05-16 124234](https://github.com/user-attachments/assets/d2901e1d-bd45-43af-b58c-3fbb4f51523b)
