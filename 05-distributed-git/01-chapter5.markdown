# Rozproszony Git #

Teraz kiedy skonfigurowałeś już zdalne repozytorium dla wszystkich programistów aby mogli się dzielić swoim kodem. Oraz jesteś zaznajomiony z podstawami poleceń w Git związanych z lokalnym schematem pracy (workflow). Zobaczysz jak korzystać z rozproszonego przepływu pracy (workflow) które oferuje Ci Git.

W ostatnim rozdziale zobaczyłeś jak pracować z gitem w rozproszonym środowisku jako współpracownik oraz osoba mająca za zadanie zintegrować wykonane prace. O to właśnie chodzi umiesz już pracować razem z innymi nad jednym kodem i robić to w sposób łatwy dla Ciebie oraz osób konserwujących go. Wiesz również jak zarządzać i utrzymywać kod pisany przez wielu programistów.

## Rozproszony schemat pracy ##

W odróżnieniu od Scentralizowanych Systemów Kontroli Wersji (CVCSs) rozproszona natura Gita pozwala być dużo bardziej elastycznym w tym jak programiści współpracują nad projektami. W scentralizowanych systemach, każdy programista jest jako węzeł pracujący bardziej lub mniej na centralnym systemie. W Gicie z kolei każdy programista jest zarówno węzłem oraz centralnym systemem - właśnie tak, każdy programista może zarówno współtworzyć kod dla innych repozytoriów. Jak i zarządzać czy konserwować publiczne repozytoria które są tworzone przez innych programistów. Takie podejście otwiera ogromny wachlarz możliwości jeśli chodzi o schemat pracy dla twojego projektu oraz drużyny. Przedstawimy tutaj kilka powszechnych paradygmatów korzystających z tej elastyczności. Ukażemy zarówno mocne jak i słabe strony każdego podejścia. Możesz wybrać jeden z nich albo mieszać i wybierać tylko pasujące możliwości.  

### Scentralizowany schemat pracy ###

W scentralizowanych systemach jest generalnie tylko jeden model współpracy - scentralizowany schemat pracy. Jeden centralny system albo repozytorium akceptuje kod i każdy synchronizuje swoją prace względem tego. Wielu programistów jest węzłami - konsumentami centralnego systemu i synchronizują się z tym (zobacz Rysunek 5-1).

Insert 18333fig0501.png 
Rysunek 5-1. Scentralizowany schemat pracy.

Oznacza to że jeśli dwóch programistów pobierze coś z centralnego systemu i obydwoje dokonają zmian. Pierwszy który wyśle swoje zmiany zrobi to bez problemu. Drugi przed wysłaniem zmian będzie musiał najpierw scalić swoją prace z pierwszym po to żeby nie nadpisać jego pracy. To podejście jest poprawne dla Gita tak samo jak dla Subversion (albo innego CVCS).

Jeżeli posiadasz małą drużynę albo jesteś już zaznajomiony z scentralizowanym schematem pracy w swojej firmie lub drużynie. Możesz bez problemu dalej stosować to podejście w Git. Po prostu utwórz pojedyncze repozytorium i daj każdemu w swojej firmie prawa do wysyłania. Git nie pozwoli użytkownikom na nadpisywanie ich prac. Jeżeli jeden z programistów sklonuje czy dokona zmian, a następnie spróbuje wysłać zmiany podczas gdy inny programista w międzyczasie wysłał swoje. Serwer odrzuci te zmiany. Otrzyma komunikat że zmiany które próbuje wysłać są non-fast-forward i że nie będzie w stanie tego zrobić dopóki wcześniej nie wykona poleceń fetch i merge. Taki schemat pracy jest atrakcyjny dla wielu ludzi, ponieważ jest on podejściem z którym wiele osób jest zaznajomiona. 

### Schemat pracy Menadżera Integracji ###

Ponieważ Git pozwala na posiadanie wielu zdalnych repozytoriów, możliwy jest taki schematu pracy gdzie każdy deweloper posiada prawa zapisu do własnego publicznego repozytorium, a do wszystkich innych tylko prawa odczytu. Taki scenariusz często zawiera kanoniczne repozytorium, które reprezentuje "oficjalny" projekt. Aby wnieść do projektu coś od siebie, trzeba stworzyć własny publiczny klon tego projektu i do niego wysyłać wszystkie zmiany. Następnie, do osoby zarządzającej głównym projektem można wysłać żądanie pociągnięcia naszych zmian. Nasze repozytorium może być dodane jako zdalne, zmiany lokalnie przetestowane, scalone i wysłane do repozytorium z głównym projektem. Proces ten przebiega następująco (patrz Rysunek 5-2):

1.	Opiekun projektu wysyła projekt do własnego, publicznego repozytorium.
2.	Osoba udzielająca się klonuje to repozytorium i wprowadza zmiany.
3.	Osoba udzielająca się wysyła zmiany do własnej publicznej kopii repozytorium.
4.	Osoba udzielająca się wysyła opiekunowi projektu wiadomość e-mail z prośbą o pobranie zmian (pull request).
5.	Opiekun projektu dodaje repozytorium osoby udzielającej się jako zdalne i scala lokalnie.
6.	Opiekun projektu wysyła scalone zmiany do głównego repozytorium.

Insert 18333fig0502.png 
Rysunek 5-2. Schemat pracy Menadżera Integracji.

Jest to bardzo powszechny schemat pracy z witrynami takimi jak GitHub, gdzie łatwo jest zrobić fork danego projektu, a następnie wysyłać do niego zmiany aby wszyscy je widzieli. Jedną z głównych zalet takiego podejścia jest to, że można spokojnie kontynuować pracę, a osoba opiekująca się głównym repozytorium będzie mogła pociągnąć zmiany w dowolnej chwili. Osoby wnoszące wkład nie muszą czekać na włączenie ich zmian do projektu — każda ze stron może pracować we własnym tempie.

### Schemat pracy Dyktatora i Poruczników ###

Jest to wariant schematu pracy Wielokrotne Repozytorium. Generalnie jest on używany przez ogromne projekty, przy których współpracują setki osób; najlepszym przykładem jest kernel Linuxa. Za poszczególne części repozytorium odpowiadają różne menadżery integracji; nazywane są porucznikami. Wszyscy porucznicy posiadają jednego menadżera integracji znanego jako życzliwy dyktator. Repozytorium życzliwego dyktatora służy jako referencyjne repozytorium, z którego wszyscy uczestnicy projektu pobierają zmiany. Proces ten działa następująco (patrz Rysunek 5-3):

1.	Regularni programiści pracują na gałęziach poświęconych ich pracy i synchronizują (rebase) to co zrobili z gałęzią master. Master jest gałęzią dyktatora.
2.	Porucznicy scalają gałęzie programistów do własnej gałęzi master.
3.	Dyktator scala gałęzie master poruczników do własnej gałęzi master.
4.	Dyktator wysyła swoją gałąź master do repozytorium referencyjnego, dzięki czemu inni programiści mogą się z nim zsynchronizować (rebase).

Insert 18333fig0503.png  
Rysunek 5-3. Schemat pracy Życzliwego Dyktatora.

Ten schemat pracy nie jest powszechny, ale może być przydatny w bardzo dużych projektach lub w środowiskach o wysoce rozbudowanej hierarchii, ponieważ pozwala on kierownikowi projektu (dyktatorowi) na przekazanie sporej ilości pracy innym oraz zbieranie dużych podzbiorów kodu w wielu punktach przed ich integracją.

Oto niektóre powszechnie używane schematy pracy możliwe do wykorzystania z rozproszonym systemem takim jak Git, ale widać, że możliwych jest wiele odmian aby dostosować swój własny, rzeczywisty schemat pracy. Teraz kiedy możesz (mam nadzieję) określić jaka kombinacja schematów pracy będzie dla Ciebie odpowiednia, omówię kilka bardziej szczegółowych przykładów jak osiągnąć rozwiązania tworzące różne schematy.

## Wnoszenie wkładu do projektu ##

Wiesz już czym są różne schematy pracy i powinieneś dość dobrze rozumieć podstawy użytkowania Git'a. W tej sekcji poznasz kilka typowych wzorców przyczyniania się do projektu.

Głównym problemem w opisie tego procesu jest to, że istnieje ogromna ilość sposobów w jaki można go zrealizować. Ponieważ Git jest bardzo elastyczny, ludzie mogą i współpracują na różne sposoby. Dlatego, problematyczne jest opisanie w jaki sposób powinieneś udzielać się w projekcie — każdy projekt jest nieco inny. Niektóre ze zmiennych jakie trzeba uwzględnić to liczba aktywnych współpracowników, wybrany schemat pracy, możliwość robienia commit'ów i ewentualnie zewnętrzne metody współpracy.

Pierwsza zmienna to ilość aktywnych współpracowników. Ilu uczestników aktywnie wnosi wkład do danego projektu w postaci kodu i jak często? W wielu przypadkach, będzie to dwóch do trzech programistów robiących po kilka commit'ów dziennie, lub ewentualnie mniej dla projektów, które są w jakiś sposób nieaktywne. Dla naprawdę dużych firm lub projektów, liczba programistów może sięgać tysięcy, z dziesiątkami lub nawet setkami poprawek wychodzącymi każdego dnia. Jest to ważne, ponieważ czym większa liczba programistów, tym więcej problemów z upewnieniem się, że nasz kod zostanie bez problemu zaaplikowany lub łatwo scalony. Zmiany, które prześlesz mogą być nieaktualne lub poważnie uszkodzone przez pracę, która była scalana kiedy ty pracowałeś albo twoje zmiany czekały na akceptację czy wprowadzenie. W jaki sposób utrzymać swój kod, tak aby był stale aktualny, a patche były poprawne?

Następną zmienną jest schemat pracy użyty w projekcie. Czy jest scentralizowany, tzn. czy każdy programista ma identyczne prawa zapisu do głównej linii kodu? Czy projekt posiada osobę opiekującą się nim lub jest zarządzany przez menagera integracji, który sprawdza wszystkie patche? Czy wszystkie patche są wnikliwie sprawdzane i potem zatwierdzone? Czy jesteś zaangażowany w ten proces? Czy ma miejsce system poruczników, a ty musisz przesyłać swoją pracę najpierw do nich?

Następnym problemem jest możliwość zapisu. Schemat pracy jaki jest potrzebny w celu wnoszenia wkładu do projektu jest zupełnie inny kiedy masz prawa zapisu do projektu i kiedy tego prawa nie masz. Jeśli nie masz prawa zapisu, to w jaki sposób akceptowana będzie wniesiona praca? Czy projekt w ogóle posiada politykę? Ile jednocześnie wnosisz pracy? Jak często to robisz?

Wszystkie te pytania mogą mieć wpływ na to jak efektywnie udzielać się w projekcie i jakie schematy pracy są preferowane lub dostępne dla ciebie. Aspekty każdego z nich zostaną omówione w serii przypadków użycia, przechodząc od prostych do bardziej złożonych. Dzięki tym przykładom powinieneś być w stanie zbudować specyficzne schematy pracy, których potrzebujesz w praktyce.

### Przewodnik po komitach ###

Zanim zaczniesz się przyglądać szczególnym przypadkom użycia, masz tutaj krótka wzmiankę o tym jak pisać wiadomości dla zatwierdzeń. Posiadanie dobrego przewodnika o tworzeniu komitów i podążanie według jego zaleceń czyni prace z Gitem i współprace z innymi dużo łatwiejszym. Projekt Gita dostarcza dokument w którym znajduje się duża ilość porad dla tworzenia komitów - możesz poczytać o tym w kodzie źródłowym Gita w pliku `Documentation/SubmittingPatches`. 

Po pierwsze, nie chcesz zatwierdzać żadnych błędów dotyczących białych znaków. Git dostarcza łatwy sposobów na sprawdzenie tego - zanim zatwierdzisz, odpal `git diff --check`, który identyfikuje możliwe błędy białych znaków i wypisze je dla Ciebie. Oto przykład gdzie zastąpiłem czerwony kolor terminala tym `X`s:

	$ git diff --check
	lib/simplegit.rb:5: trailing whitespace.
	+    @git_dir = File.expand_path(git_dir)XX
	lib/simplegit.rb:7: trailing whitespace.
	+ XXXXXXXXXXX
	lib/simplegit.rb:26: trailing whitespace.
	+    def command(git_cmd)XXXX

Jak przed zatwierdzeniem odpalisz to polecenie, zostaniesz poinformowany czy masz zamiar zatwierdzić problem z białymi znaki które mogą denerwować innych programistów.

Następnie spróbuj stworzyć każdy komit tak aby był od siebie logicznie oddzielony. Jeżeli chcesz spróbuj utworzyć zmiany tak aby były łatwe do przyswojenia dla innych - nie pisz kodu przez cały weekend dla pięciu różnych zagadnień a następnie wysyłaj je wszystkie jako jedno ogromne zatwierdzenie w poniedziałek. Nawet jeżeli nie tworzysz zatwierdzeń w weekend podziel prace którą wykonałeś na co najmniej jedno zatwierdzenie dla zagadnienia, z użyteczną wiadomością dla danego zatwierdzenia. Jeżeli niektóre ze zmian modyfikują ten sam plik , postaraj się użyć polecenia `git add --patch` aby częściowo rozdzielić pliki (opisane dokładnie w rozdziale 6). Migawka projektu na górze gałęzi jest identyczna niezależnie od tego czy zrobisz jeden komit czy pięć, tak długo jak wszystkie zmiany będą dodane w tym samym punkcie. Dlatego poprzez stosowanie się do tych wytycznych ułatwisz sprawę programistą którzy później będą przeglądać twoje zmiany. Takie podejście również ułatwia pobierania albo odwracanie zmian jeżeli później zajdzie taka potrzeba. Rozdział 6 opisuje różne użyteczne porady na poprawianie historii i interaktywne poziomowanie plików - używaj tych narzędzi aby ułatwić wykonywanie czystej i zrozumiałem historii.     

Ostatnią rzeczą na którą trzeba zwrócić uwagę to wiadomości wpisywane przy komitach. Posiadanie w zwyczaju tworzenia wartościowych wiadomości dla komitów czyni używanie i współprace z Gitem wiele łatwiejszym. Jako ogólna zasada przyjmij że twoje wiadomości powinny zaczynać się jako pojedyńcza linia która nie posiada więcej niż 50 znaków i opisuje krótko zbiór zmian jakich dokonałeś. Następnie pusta linia i bardziej szczegółowe wyjaśnienie. Projekt Gita wymaga aby szczegółowe wyjaśnienia zawierały twoją przyczynę zmian i skontrastowały ją z poprzednią wersją - jest to dobry przewodnik do którego powinieneś się stosować. Dobrym pomysłem jest również używanie czasu rozkazującego w wiadomościach. Innymi słowy używaj komend. Zamiast np "Dodałem testy dla" lub "Dodaje testy dla" użyj "Dodane testy dla".

Oto szablon napisany przez Tim Pope na tpope.net:   

	Krótkie )50 znaków lub mniej) zestawienie zmian

	Bardziej szczegółowie teksty wyjaśniające, jeśli to konieczne. Zwiń go
	do 72 znaków, lub więcej. W niektórych sytuacjach pierwsza linia jest traktowana jako
	temat wiadomości e-mail, a reszta tekstu jako ciała. Pusta linia oddzielająca
	podsumowanie od ciała ma kluczowe znaczenie (o ile ciało nie zostanie pominięte
	całkowicie); narzędzia takie ja rebase mogą się mylić, jeśli uruchomisz
	dwa razem.

	Kolejne paragrafy przejdą do pustych linii.

	 - Wybunktowanie jesy w porządku

	 - Zazwyczaj do wypunktowania używana jest gwiazdka lub myślnik, poprzedzone
	   pustym  miejscem i pustymi liniami między nimi, ale konwencje mogą się różnić

Jeśli wszystkie commity wiadomości będą tak wyglądały to wszystko będzie dużo łatwiejsze dla developerów z którymi pracujesz. Projekt Git ma dobrze sformatowane commity widaomości - Zachęcam uruchomić `git log --no-merges` aby zobaczyć jak wygląda historia ładnie sformatowanych commitów.

W poniższych przykładach, a więc przez większość książki używam 'git commit' z opcją '-M'. Nie rób tego ta jak ja, rób tak jak mówię.

### Mały prwatny zespół ###

Najprostszym ustawieniem z którym się prawdopodobnie spotkasz to prywatny projekt z jednym lub dwoma programistami. Poprzez prywatny mam na myśli zamknięty kod - bez możliwości odczytu dla reszty świata. Ty oraz inni programiści musicie wszyscy wysłać dostęp do repozytorium.

W tym środowisku, możesz podążać według schematu pracy podobnego do tego, który wykorzystywany jest w Subversion lub innych scentralizowanych systemach. Wciąż zyskasz przewagę takich rzeczy jak zatwierdzanie w trybie offline i dużo łatwiejsze gałęzie oraz złączenia, ale ten schemat pracy może być dużo łatwiejszy. Główną różnicą jest to że złączenia wykonują się po stronie klienta zamiast po stronie servera w czasie commitów.
Zobacz jak może wyglądać gdy dwóch programistów rozpoczyna wspólną prace w dzielonym repozytorium. Pierwszy programista, John klonuje repozytorium dokonuje zmiany i zatwierdza lokalnie. (W tym przykładzie zastąpię wiadomość protokołu tym `...`  aby go skrócić).

	# John's Machine
	$ git clone john@githost:simplegit.git
	Initialized empty Git repository in /home/john/simplegit/.git/
	...
	$ cd simplegit/
	$ vim lib/simplegit.rb 
	$ git commit -am 'removed invalid default value'
	[master 738ee87] removed invalid default value
	 1 files changed, 1 insertions(+), 1 deletions(-)

Drugi programista, Jessica, robi to samo - klonuje repozytorium i zatwierdza zmiany:

	# Jessica's Machine
	$ git clone jessica@githost:simplegit.git
	Initialized empty Git repository in /home/jessica/simplegit/.git/
	...
	$ cd simplegit/
	$ vim TODO 
	$ git commit -am 'add reset task'
	[master fbff5bc] add reset task
	 1 files changed, 1 insertions(+), 0 deletions(-)

Teraz, Jessica wysyła swoją prace na server:

	# Jessica's Machine
	$ git push origin master
	...
	To jessica@githost:simplegit.git
	   1edee6b..fbff5bc  master -> master

John próbuje wysłać swoje zmiany również:

	# John's Machine
	$ git push origin master
	To john@githost:simplegit.git
	 ! [rejected]        master -> master (non-fast forward)
	error: failed to push some refs to 'john@githost:simplegit.git'

John nie otrzymał zezwolenia na wysłanie, ponieważ w międzyczasie Jessica wysłała swoje. Jest to szczególnie ważne do zrozumienia jeśli jesteś przyzwyczajony do Subversion, ponieważ zauważ ze dwóch programistów nie edytuje tego samego pliku. Chociaż Subversion automatycznie tworzy złączenia na serwerze jeśli edytowane są różne pliki. W Gicie musisz połączyć zatwierdzenia lokalnie. John musi sprowadzić zmiany Jessic'i i połączyć je ze swoimi zanim dostanie pozwolenie na wysłanie tego na serwer.	

	$ git fetch origin
	...
	From john@githost:simplegit
	 + 049d078...fbff5bc master     -> origin/master

W tym momencie lokalne repozytorium Johna wygląda mniej wiecej w ten sposób Zdjęcie 5-4.

Insert 18333fig0504.png 
Zdjęcie 5-4. Repozytorium Johna

John ma odwołanie do zmian wysłanych przez Jessice, ale musi on połączyć to wszystko zanim będzie mógł wysłać na sewer.

	$ git merge origin/master
	Merge made by recursive.
	 TODO |    1 +
	 1 files changed, 1 insertions(+), 0 deletions(-)

Scalanie poszło gładko — historia zatwierdzeń Johna wygląda jak na Rysunku 5-5.

Insert 18333fig0505.png 
Figure 5-5. Repozytorium Johna po scaleniu żródła/gałąź główna.

Teraz, John moze sprawdzić swój kod czy nadal działa poprawnie, a następnie może wysłać swoją nową prace do repozytorium:

	$ git push origin master
	...
	To john@githost:simplegit.git
	   fbff5bc..72bbc59  master -> master

W końcu, historia zatwierdzeń Johna wygląda jak na Rysunku 5-6.

Insert 18333fig0506.png 
Figure 5-6. Historia Johna po wysłaniu swojej pracy na serwer źródło.

W miedzyczasie, Jessica pracuje nad tematyczną gałęzią. Tworzy nową gałąź nazwaną `issue54` i robi trzy zatwierdzenia do tej gałęzi. Nie rozgałęziła ona jeszcze zmian Johna więc jej historia wygląda jak na Rysunku 5-7.

Insert 18333fig0507.png 
Figure 5-7. Historia inicjacji zatwierdzeń Jessica'i.

Jessica chce zsynchronizować się z Johnem, więc wykonuje rozgałęzienie:

	# Jessica's Machine
	$ git fetch origin
	...
	From jessica@githost:simplegit
	   fbff5bc..72bbc59  master     -> origin/master

To pozwoli na ściągnięcie pracy, którą John wysłał na serwer. Historia Jessica'i wygląda teraz jak na Rysunku 5-8.

Insert 18333fig0508.png 
Figure 5-8. Historia Jessica'i po rozgałęzieniu pracy Johna.

Jessica myśli, że jej gałąź tematyczna jest gotowa, ale chce wiedzieć co musi scalić do swojej gałezi aby wysłać ją na serwer. Uruchamia `git log` aby się dowiedzieć:

	$ git log --no-merges origin/master ^issue54
	commit 738ee872852dfaa9d6634e0dea7a324040193016
	Author: John Smith <jsmith@example.com>
	Date:   Fri May 29 16:01:27 2009 -0700

	    removed invalid default value

Teraz, Jessica może scalić swoja gałąź tematyczną z gałęzią główną, scalić pracę Johna (`origin/master`) do jej gałezi `master`, i wysłać wszystko na serwer jeszcze raz. Najpierw, przełącza się do swojej gałezi głównej aby zintegrować pracę:

	$ git checkout master
	Switched to branch "master"
	Your branch is behind 'origin/master' by 2 commits, and can be fast-forwarded.

Może scalić obie `origin/master` lub `issue54` najpierw — obydwie gałęzie są śledzone, więc kolejnosć nie ma znaczenia. Końcowa wersja plików powinna być identyczna bez względu na to jaką kolejnosć wybierze; tylko historia będzie nieco różna. Wybrała scalenie najpierw `issue54`:

	$ git merge issue54
	Updating fbff5bc..4af4298
	Fast forward
	 README           |    1 +
	 lib/simplegit.rb |    6 +++++-
	 2 files changed, 6 insertions(+), 1 deletions(-)

Żaden prblem nie wystapił; jak widzicie, był to prosty sposób na szybką komunikacje. Teraz Jessica scala prace Johna (`origin/master`):

	$ git merge origin/master
	Auto-merging lib/simplegit.rb
	Merge made by recursive.
	 lib/simplegit.rb |    2 +-
	 1 files changed, 1 insertions(+), 1 deletions(-)

Wszystko scaliło się bez błędnie, a historia Jessica'i wygląda jak na Rysunku 5-9.

Insert 18333fig0509.png 
Figure 5-9. Historia Jessica'i po scaleniu pracy Johna.

Teraz `origin/master` jest osiągalny z gałęzi `master` Jessicsa'i, więc może teraz z powodzeniem wysłać na serwer (Jeśli oczywiście John nie wyśle czegoś na serwer wcześniej):

	$ git push origin master
	...
	To jessica@githost:simplegit.git
	   72bbc59..8059c15  master -> master

Paru deweloperów zatwierdzało parę razy i scalało swoją pracę z sukcesem; zobacz Rysunek 5-10.

Insert 18333fig0510.png 
Zdjęcie 5-10. Historia Jessica'i po tym jak wysłała wszystko na serwer.

To jest jeden z najprostrzych schematów pracy. Pracujesz przez chwilę na tematycznej gałęzi, a następnie scalasz je ze swoją gałęzią główną po tym jak będą gotowe do zintegrowania. Kiedy bedziesz chciał udostępnic swoją pracę, scalasz ją ze swoja gałęzią główną, następnie rozgałęziasz i scalasz `origin/master` jesli ma zmiany, a na końcu wysyłasz wszystko do gałęzi `master` na serwerze. Schemat tej operacji przedstawiono na Rysunku 5-11.

Insert 18333fig0511.png 
Zdjęcie 5-11. Prosty schemat pracy kilku deweloperów krok po kroku.

### Prywatny zarządzany zespół  ###

W następnym przykładzie, spojrzysz na role osoby wspierającej w dużej prywatnej grupie. Nauczysz się jak pracować w środowisku gdzie małe grupy współpracują nad możliwościami i Ci którzy wspierają dany zespół są zintegrowanie przez inną drużynę.

Powiedzmy że John i Jessica pracują razem nad jakąś funkcją. Podczas gdy Jessica i Josie pracują nad inną. W tym przypadku, firma używa typu zintegrowanego managera schematu pracy gdzie praca pojedynczej grupy jest integrowana tylko poprzez pewnych inżynierów. Gałąź master głównego repozytorium może być aktualizowana tylko przez tych inżynierów. W tym scenariuszu cała praca jest wykonana w gałęziach przydzielonych tylko dla danego zespołu i wysyłana jest razem przez osobę integrującą.

Podążajmy za schematem pracy Jessica'i gdy pracuje nad swoimi dwoma funkcjami. Współpracuje ona równoległe z dwoma różnymi programistami w tym samym środowisku. Zakładając że posiada już sklonowane własne repozytorium zdecydowała że rozpocznie pracy od `featureA` jako pierwszej.

	# Jessica's Machine
	$ git checkout -b featureA
	Switched to a new branch "featureA"
	$ vim lib/simplegit.rb
	$ git commit -am 'add limit to log function'
	[featureA 3300904] add limit to log function
	 1 files changed, 1 insertions(+), 1 deletions(-)

W tym punkcie potrzebuje podzielić się swoją pracą z Johnem dlatego wysyła na serwer swoje zatwierdzenie gałęzi z funkcją `featureA`. Jessica nie musi mieć dostępu do wysyłania dla gałęzi master - tylko integratorzy mogą to zrobić - dlatego musi ona wysłać to do innej gałęzi po to żeby mieć możliwość współpracy z Johnem.

	$ git push origin featureA
	...
	To jessica@githost:simplegit.git
	 * [new branch]      featureA -> featureA

Jessica informuje mailem Johna że wysłała efekty swojej pracy do gałęzi o nazwie `featureA` i może on je teraz zobaczyć. Podczas gdy czeka na jego odzew, decyduje się rozpocząć pracę nad `featureB` z Josie. Aby rozpocząć tworzy nową gałąź znajdującą się na serwerze z dala od gałęzi `master`.

	# Jessica's Machine
	$ git fetch origin
	$ git checkout -b featureB origin/master
	Switched to a new branch "featureB"

Teraz Jessica tworzy kilka commitów w gałęzi `featureB`

	$ vim lib/simplegit.rb
	$ git commit -am 'made the ls-tree function recursive'
	[featureB e5b0fdc] made the ls-tree function recursive
	 1 files changed, 1 insertions(+), 1 deletions(-)
	$ vim lib/simplegit.rb
	$ git commit -am 'add ls-files'
	[featureB 8512791] add ls-files
	 1 files changed, 5 insertions(+), 0 deletions(-)

Repozytorium Jessici wygląda jak Zdjęciu 5-12.

Insert 18333fig0512.png 
Zdjęcie 5-12. Początkowa historia zatwierdzeń Jessici.

Ona jest gotowa aby wysłać swoją pracę, ale dostaje emaila od Josie że gałąź w której została wykonana pewna praca została już wysłana na serwer jako `featureBee`. Jessica po pierwsze potrzebuje scalić te zmiany ze swoimi zanim będzie mogła wysłać je na serwer. Ona może pobrać zmiany Josie'go poleceniem `git fetch`. 

	$ git fetch origin
	...
	From jessica@githost:simplegit
	 * [new branch]      featureBee -> origin/featureBee

Jessica teraz może zcalić to ze swoimi zmianami polecneim `git merge`:

	$ git merge origin/featureBee
	Auto-merging lib/simplegit.rb
	Merge made by recursive.
	 lib/simplegit.rb |    4 ++++
	 1 files changed, 4 insertions(+), 0 deletions(-)

Jest mały problem - potrzebuje ona wysłać swoją scaloną prace w gałęzi `featureB` do gałęzi `featureBee` na serwerze. Może zrobić to poprzez określenia w poleceniu `git push` lokalnej gałęzi po której znajduje się dwukropek (:) i nazwa gałęzi na serwerze.

	$ git push origin featureB:featureBee
	...
	To jessica@githost:simplegit.git
	   fba9af8..cd685d1  featureB -> featureBee

Nazywane jest to _refspec_. Zobacz rozdział 9 aby uzyskać bardziej szczegółową dyskusje o refspacs w Gicie i innych różnych rzeczach które możesz z nimi zrobić.

Następnie John wysyła email do Jessici aby poinformować ją że wysłał pewne zmiany do gałęzi `featureA` i prosi ją o zweryfikowanie ich. Ona odpala `git fetch` aby pobrać te zmiany.

	$ git fetch origin
	...
	From jessica@githost:simplegit
	   3300904..aad881d  featureA   -> origin/featureA

Następnie widzi co zostało zmienione poleceniem `git log`:

	$ git log origin/featureA ^featureA
	commit aad881d154acdaeb2b6b18ea0e827ed8a6d671e6
	Author: John Smith <jsmith@example.com>
	Date:   Fri May 29 19:57:33 2009 -0700

	    changed log output to 30 from 25

Ostatecznie łączy prace Johna ze swoją w gałęzi `featureA`:

	$ git checkout featureA
	Switched to branch "featureA"
	$ git merge origin/featureA
	Updating 3300904..aad881d
	Fast forward
	 lib/simplegit.rb |   10 +++++++++-
	1 files changed, 9 insertions(+), 1 deletions(-)
	
Jessica chce dopracować coś więc zatwierdza ponownie i wysyła tą kopie zapasową na serwer:

	$ git commit -am 'small tweak'
	[featureA ed774b3] small tweak
	 1 files changed, 1 insertions(+), 1 deletions(-)
	$ git push origin featureA
	...
	To jessica@githost:simplegit.git
	   3300904..ed774b3  featureA -> featureA

Historia zatwierdzeń Jessici wygląda teraz następująco Zdjęcie 5-13.

Insert 18333fig0513.png
Zdjęcie 5-13. Historia Jessici po zatwierdzeniu właściwej gałęzi.

Jessica, Josie i John informują osobę integrującą że gałęzie `featureA` and `featureBee` są gotowe do zintegrowania z głównym projektem. Po tym jak gałęzi zostaną włączone do głównego projektu, pobranie spowoduje nowe scalenie zatwierdzeń, tworząc historie zatwierdzeń podobną do Zdjęcia 5-14.

Insert 18333fig0514.png 
Zdjęcie 5-14. Historia pracy Jessici po scaleniu jej obydwu tematycznych gałęzi.

Wiele grup przechodzi na Gita ponieważ umożliwia on współprace kilku zespołom jednocześnie, pozwalając na późniejsze scalanie różnych linie pracy. Możliwość współpracy mniejszych podgrup jakiegoś zespołu poprzez zdalne gałęzie bez konieczności wciągania w to całego zespołu jest ogromną zaletą Gita. Sekwencja schematu pracy którą tutaj zobaczyłeś wygląda jak na zdjęciu 5-15. 

Insert 18333fig0515.png 
Zdjęcie 5-15. Prosta sekwencja schematu pracy managed-team.

### Publiczne małe projekty ###

Wnoszenie wykładu do publicznych projektów jest trochę inne, ponieważ nie masz uprawnień na bezpośrednie aktualizowanie gałęzi. Musisz przenieść swoją prace do osób zarządzających tym projektem w jakiś inny sposób. Ten przykład opisuje wnoszenie wkładu poprzez rozwidlenia "forking" na hostach Gita który wspiera proste rozwidlenia "easy forking". Strony takie jak repo.or.cz i GitHub hosting wspierają tą funkcje i wiele osób zarządzających swoimi projektami oczekuje że będziesz w ten sposób dzielił się z nimi swoją pracą.

Po pierwsze będziesz prawdopodobnie chciał sklonować repozytorium, stworzyć gałąź dla łatki albo serii łatek którymi planujesz się podzielić, i wykonać swoją prace w niej. Sekwencja wygląda podobnie jak to: 

	$ git clone (url)
	$ cd project
	$ git checkout -b featureA
	$ (work)
	$ git commit
	$ (work)
	$ git commit

Możesz użyć polecenia `rebase -i` aby wepchnąć całą swoją prace do pojedynczego komita lub re-aranżować prace w komitch tak aby stworzyć łatkę łatwiejszą do sprawdzenia dla osoby zarządzającej.  

Kiedy twoja gałąź jest ukończona i jesteś gotowy wysłać ją z powrotem dla osób zarządzających projektem. Przejdź do strony oryginalnego projektu i kliknij przycisk "Fork", tworząc w ten sposób swoje własne rozwidlenie dla projektu. Możesz wtedy też dodać do repozytorium drugi zdalny URL w tym przypadku `myfork`: 

	$ git remote add myfork (url)

Musisz wysłać swoją prace do niego. Łatwiej jest wysłać zdalną gałąź nad którą pracujesz do repozytorium, niż scalić wszytko do gałęzi master i dopiero to wysłać.
Powodem który przemawia za tym jest to że jeżeli twoja praca nie zostanie zaakceptowana lub popełniłeś jakiś błąd logiczny nie musisz przerabiać gałęzi master. Jeżeli osoby zarządzające scalą, "rebase" lub znajdą jakieś błędy w twojej pracy będziesz mógł dostać wszytko z powrotem poprzez pobranie tego z ich repozytorium.

	$ git push myfork featureA

Kiedy twoja praca została wysłana do rozwidlenia musisz powiadomić osobę zarządzającą. Często jest to nazywane "pull request" i możesz też wygenerować to poprzez stronę - GitHub posiada przycisk "pull request" który automatycznie powiadamia osobę zarządzającą - lub odpalenie polecenia `git request-pull` i manualne wysłanie maila do kierownika.  

Polecenie `request-pull` bierze główną gałąź do której chcesz aby twoja gałąź została wysłana oraz URL repozytorium Gita z którego chcesz je pobrać. Dodatkowo bierze podsumowanie wszystkich zmian o które pytasz aby zostały pobrane. Na przykład jeśli Jessica chce wysłać Johnowi pull request i wykonała dwa komity na gałęzi którą właśnie pobrała może odpalić to:  

	$ git request-pull origin/master myfork
	The following changes since commit 1edee6b1d61823a2de3b09c160d7080b8d1b3a40:
	  John Smith (1):
	        added a new function

	are available in the git repository at:

	  git://githost/simplegit.git featureA

	Jessica Smith (2):
	      add limit to log function
	      change log output to 30 from 25

	 lib/simplegit.rb |   10 +++++++++-
	 1 files changed, 9 insertions(+), 1 deletions(-)

Wyjście może zostać wysłane do kierownika - poinformuje go to z której gałęzi pochodzi praca, dokona podsumowania komitów oraz powie skąd pobrać daną prace.

Dla projektów w których nie jesteś kierownikiem, łatwiej jest mieć gałęzie takie jak `master` zawsze śledzące `origin/master` i robić swoje zadania w tematycznych gałęziach których możesz pozbyć się jeżeli są odrzucone. Posiadanie motywów pracy w wyizolowanych tematycznych gałęziach również ułatwia Ci "rebase" twojej pracy jeśli koniec głównego repozytorium został w międzyczasie przemieszczony i twoje komity nie będą czysto dodawane.

	$ git checkout -b featureB origin/master
	$ (work)
	$ git commit
	$ git push myfork featureB
	$ (email maintainer)
	$ git fetch origin

Teraz każdy z tematów jest zawarty wewnątrz silo - coś podobnego do kolejki łatek - które możesz przepisać, rebase lub zmodyfikować bez mieszania tematów lub tworzenia zależności pomiędzy nimi jak na zdjęciu 5-16.

Insert 18333fig0516.png 
Zdjęcie 5-16. Wstępna historia komitów z pracą featureB.

Powiedzmy że kierownik projektu pobrał wiele różnych łatek i najpierw spróbował twojej pierwszej gałęzi która nie scala się czysto z resztą. W tym przypadku możesz spróbować "rebase" tej gałęzi na górze `origin/master`, następnie rozwiązać konflikt z kierownikiem i ponownie zatwierdzić zmiany:

	$ git checkout featureA
	$ git rebase origin/master
	$ git push -f myfork featureA

To przepisuje twoją historie która teraz wygląda jak na zdjęciu 5-17.

Insert 18333fig0517.png 
Zdjęcie 5-17. Historia komitów po pracy nad featureA.

Ponieważ właśnie scaliłeś poprzez (rebase) swoją gałąź. Musisz określić atrybut `-f` dla polecenia push po to żeby być w stanie zastąpić gałąź `featureA` na serwerze
komitem który nie jest jego potomkiem. Alternatywą było by wysłanie swojej pracy do innej gałęzi na serwerze (np nazwanej `featureAv2` ).

Spójrzmy na jeden możliwy scenariusz: kierownik przyjrzał się pracy w twojej drugiej gałęzi i podoba mu się twój pogląd jednak chciałby zmienić szczegóły implementacji. 
Ty również korzystasz z tej okoliczności aby przenieść swoją prace tak aby bazowała na obecnej gałęzi `master` projektu. Rozpoczynasz nową gałąź bazującą na obecnej gałęzi `origin/master`, wplatasz tam zmiany z `featureB`, rozwiązujesz konflikty, dokonujesz zmiany implementacji i wysyłasz to do gałęzi:   

	$ git checkout -b featureBv2 origin/master
	$ git merge --no-commit --squash featureB
	$ (change implementation)
	$ git commit
	$ git push myfork featureBv2

Opcja `--squash` bierze całą prace w scalonej gałęzi i wplata ją do niescalonego komita na szczycie gałęzi na której obecnie jesteś. Opcja `--no-commit` informuje Gita żeby automatycznie nagrywał komity. Pozwala Ci to sprowadzić wszystkie zmiany z innej gałęzi i dokonać kolejnych zmian przed zapisaniem do nowego komita.

Teraz możesz wysłać kierownikowi wiadomość że dokonałeś zmian o które prosił i mogą oni je znaleźć w gałęzi `featureBv2` (zobacz Zdjęcie 5-18).

Insert 18333fig0518.png 
Figure 5-18. Historia komitów po pracy featureBv2.

### Publiczne duże projekty ###

Wiele większych projektów ma określone procedury dla akceptacji łatek - będziesz potrzebował sprawdzić jakie mają reguły dla każdego projektu, ponieważ różnią się one między sobą. Jednak wiele większych projektów akceptuje łatki poprzez mailową listę programistów, więc przeanalizujemy pewien przykład teraz.

Szablon pracy jest podobny do użytego w poprzednim przykładzie - możesz tworzyć tematyczne gałęzie dla każdej serii łatek nad którą pracujesz. Różnica to w jaki sposób będziesz zatwierdzał je do projektu. Zamiast tworzyć rozgałęzienie w projekcie i wysyłać swoją prace do własnej wersji. Teraz wygenerujesz wersje maili dla każdej serii zatwierdzeń i wyślesz to do listy maili programistów.

	$ git checkout -b topicA
	$ (work)
	$ git commit
	$ (work)
	$ git commit

Teraz masz dwa komity które chcesz wysłać na listę mailową. Użyjesz polecenia `git format-patch` aby wygenerować mbox-sformatowane pliki które możesz wysłać mailem na liste - zamienia to każdego komita w wiadomość email gdzie pierwsza linia wiadomości dotyczącej zatwierdzenia jest tematem natomiast reszta ciałem. Miłą rzeczą odnośnie tego jest to że dodawanie łatek z maila wygenerowanego z `format-patch` chroni wszystkie informacje o zatwierdzeniu. Będziesz mógł się o tym przekonać w kolejnej sekcji gdzie będziemy dodawać te komity.  

	$ git format-patch -M origin/master
	0001-add-limit-to-log-function.patch
	0002-changed-log-output-to-30-from-25.patch

Polecenie `format-patch` drukuje nazwy plików z tych łatek które tworzy. Przełącznik `-M` mówi Gitowi żeby szukać zmian nazw. Pliki kończą się w ten sposób:

	$ cat 0001-add-limit-to-log-function.patch 
	From 330090432754092d704da8e76ca5c05c198e71a8 Mon Sep 17 00:00:00 2001
	From: Jessica Smith <jessica@example.com>
	Date: Sun, 6 Apr 2008 10:17:23 -0700
	Subject: [PATCH 1/2] add limit to log function

	Limit log functionality to the first 20

	---
	 lib/simplegit.rb |    2 +-
	 1 files changed, 1 insertions(+), 1 deletions(-)

	diff --git a/lib/simplegit.rb b/lib/simplegit.rb
	index 76f47bc..f9815f1 100644
	--- a/lib/simplegit.rb
	+++ b/lib/simplegit.rb
	@@ -14,7 +14,7 @@ class SimpleGit
	   end

	   def log(treeish = 'master')
	-    command("git log #{treeish}")
	+    command("git log -n 20 #{treeish}")
	   end

	   def ls_tree(treeish = 'master')
	-- 
	1.6.2.rc1.20.g8c5b.dirty

Możesz również edytować pliki łatki, aby dodać więcej informacji dla listy email które nie chcesz pokazywać w wiadomościach dotyczących komitów. Jeżeli dodasz tekst pomiędzy linią `--` i początkiem łatki (linia `lib/simplegit.rb`) wtedy programiści mogą to przeczytać ale dołączenie łatki wyklucza to.

Aby wysłać to do listy maili możesz zarówno wkleić swój plik w program mailowy lub wysłać to poprzez linie poleceń programu. Przy wklejaniu występują często problemy z formatowaniem szczególnie z "mądrzejszymi" klientami które nie zapewniają odpowiedniej ochrony przed nowymi liniami i innymi białymi znakami. Na szczęście Git dostarcza narzędzia które pomogą Ci wysyłać odpowiednio sformatowane łatki poprzez IMAP, co możesz być dla Ciebie dużo łatwiejsze. Zademonstruje jak wysłać łatkę poprzez Gmail, ponieważ sam go używam. Jeśli chcesz możesz przeczytać szczegółową instrukcje dla różnych programów pocztowych na końcu pliku `Documentation/SubmittingPatches` w kodzie źródłowym Gita. 

Po pierwsze potrzebujesz założyć sekcje IMAP w swoim pliku `~/.gitconfig`. Możesz ustawić każdą wartość oddzielnie serią komend `git config` lub możesz dodać je ręcznie. Ale na koniec twój plik konfiguracyjny powinien wyglądać w ten sposób.

	[imap]
	  folder = "[Gmail]/Drafts"
	  host = imaps://imap.gmail.com
	  user = user@gmail.com
	  pass = p4ssw0rd
	  port = 993
	  sslverify = false

Jeśli twój serwer IMAP nie używa SSL, ostatnie dwie linie prawdopodobnie nie są konieczne, wartość hosta będzie `imap://` zamiast `imaps://`. Jak już to ustawisz możesz użyć polecenia `git send-email` aby zamieścić serie łątek w folderze Drafts na określonym serwerze IMAP

	$ git send-email *.patch
	0001-added-limit-to-log-function.patch
	0002-changed-log-output-to-30-from-25.patch
	Who should the emails appear to be from? [Jessica Smith <jessica@example.com>] 
	Emails will be sent from: Jessica Smith <jessica@example.com>
	Who should the emails be sent to? jessica@example.com
	Message-ID to be used as In-Reply-To for the first email? y

Następnie Git rozdzieli wiązkę informacji z loga szukając czegoś takiego dla każdej łatki którą wysyłasz:

	(mbox) Adding cc: Jessica Smith <jessica@example.com> from 
	  \line 'From: Jessica Smith <jessica@example.com>'
	OK. Log says:
	Sendmail: /usr/sbin/sendmail -i jessica@example.com
	From: Jessica Smith <jessica@example.com>
	To: jessica@example.com
	Subject: [PATCH 1/2] added limit to log function
	Date: Sat, 30 May 2009 13:29:15 -0700
	Message-Id: <1243715356-61726-1-git-send-email-jessica@example.com>
	X-Mailer: git-send-email 1.6.2.rc1.20.g8c5b.dirty
	In-Reply-To: <y>
	References: <y>

	Result: OK

W tym momencie powinieneś być w stanie przejść do folder Drafts, zmienić to pole na listę maili do której wysyłasz łatki. Następnie prawdopodobnie kierownik CC lub osoba odpowiedzialna za daną sekcje odeśle Ci to.

### Podsumowanie ###

Ta sekcja zawiera wiele popularnych schematów pracy służących do obsługi różnych rodzajów projektów Gita z którymi prawodopodbnie się spotkasz. Przedstawia Ci również kilka nowych narzędzi które pomagają Ci zarządzać danym procesem. Następnie zobaczysz jak pracować po drugiej stronie monety: zarządzać projektem Gita. Nauczysz się jak być życzliwym dyktatorem albo kierownikiem integrującym.

## Maintaining a Project ##

In addition to knowing how to effectively contribute to a project, you’ll likely need to know how to maintain one. This can consist of accepting and applying patches generated via `format-patch` and e-mailed to you, or integrating changes in remote branches for repositories you’ve added as remotes to your project. Whether you maintain a canonical repository or want to help by verifying or approving patches, you need to know how to accept work in a way that is clearest for other contributors and sustainable by you over the long run.

### Working in Topic Branches ###

When you’re thinking of integrating new work, it’s generally a good idea to try it out in a topic branch — a temporary branch specifically made to try out that new work. This way, it’s easy to tweak a patch individually and leave it if it’s not working until you have time to come back to it. If you create a simple branch name based on the theme of the work you’re going to try, such as `ruby_client` or something similarly descriptive, you can easily remember it if you have to abandon it for a while and come back later. The maintainer of the Git project tends to namespace these branches as well — such as `sc/ruby_client`, where `sc` is short for the person who contributed the work. 
As you’ll remember, you can create the branch based off your master branch like this:

	$ git branch sc/ruby_client master

Or, if you want to also switch to it immediately, you can use the `checkout -b` option:

	$ git checkout -b sc/ruby_client master

Now you’re ready to add your contributed work into this topic branch and determine if you want to merge it into your longer-term branches.

### Applying Patches from E-mail ###

If you receive a patch over e-mail that you need to integrate into your project, you need to apply the patch in your topic branch to evaluate it. There are two ways to apply an e-mailed patch: with `git apply` or with `git am`.

#### Applying a Patch with apply ####

If you received the patch from someone who generated it with the `git diff` or a Unix `diff` command, you can apply it with the `git apply` command. Assuming you saved the patch at `/tmp/patch-ruby-client.patch`, you can apply the patch like this:

	$ git apply /tmp/patch-ruby-client.patch

This modifies the files in your working directory. It’s almost identical to running a `patch -p1` command to apply the patch, although it’s more paranoid and accepts fewer fuzzy matches than patch. It also handles file adds, deletes, and renames if they’re described in the `git diff` format, which `patch` won’t do. Finally, `git apply` is an "apply all or abort all" model where either everything is applied or nothing is, whereas `patch` can partially apply patchfiles, leaving your working directory in a weird state. `git apply` is overall much more paranoid than `patch`. It won’t create a commit for you — after running it, you must stage and commit the changes introduced manually.

You can also use git apply to see if a patch applies cleanly before you try actually applying it — you can run `git apply --check` with the patch:

	$ git apply --check 0001-seeing-if-this-helps-the-gem.patch 
	error: patch failed: ticgit.gemspec:1
	error: ticgit.gemspec: patch does not apply

If there is no output, then the patch should apply cleanly. This command also exits with a non-zero status if the check fails, so you can use it in scripts if you want.

#### Applying a Patch with am ####

If the contributor is a Git user and was good enough to use the `format-patch` command to generate their patch, then your job is easier because the patch contains author information and a commit message for you. If you can, encourage your contributors to use `format-patch` instead of `diff` to generate patches for you. You should only have to use `git apply` for legacy patches and things like that.

To apply a patch generated by `format-patch`, you use `git am`. Technically, `git am` is built to read an mbox file, which is a simple, plain-text format for storing one or more e-mail messages in one text file. It looks something like this:

	From 330090432754092d704da8e76ca5c05c198e71a8 Mon Sep 17 00:00:00 2001
	From: Jessica Smith <jessica@example.com>
	Date: Sun, 6 Apr 2008 10:17:23 -0700
	Subject: [PATCH 1/2] add limit to log function

	Limit log functionality to the first 20

This is the beginning of the output of the format-patch command that you saw in the previous section. This is also a valid mbox e-mail format. If someone has e-mailed you the patch properly using git send-email, and you download that into an mbox format, then you can point git am to that mbox file, and it will start applying all the patches it sees. If you run a mail client that can save several e-mails out in mbox format, you can save entire patch series into a file and then use git am to apply them one at a time. 

However, if someone uploaded a patch file generated via `format-patch` to a ticketing system or something similar, you can save the file locally and then pass that file saved on your disk to `git am` to apply it:

	$ git am 0001-limit-log-function.patch 
	Applying: add limit to log function

You can see that it applied cleanly and automatically created the new commit for you. The author information is taken from the e-mail’s `From` and `Date` headers, and the message of the commit is taken from the `Subject` and body (before the patch) of the e-mail. For example, if this patch was applied from the mbox example I just showed, the commit generated would look something like this:

	$ git log --pretty=fuller -1
	commit 6c5e70b984a60b3cecd395edd5b48a7575bf58e0
	Author:     Jessica Smith <jessica@example.com>
	AuthorDate: Sun Apr 6 10:17:23 2008 -0700
	Commit:     Scott Chacon <schacon@gmail.com>
	CommitDate: Thu Apr 9 09:19:06 2009 -0700

	   add limit to log function

	   Limit log functionality to the first 20

The `Commit` information indicates the person who applied the patch and the time it was applied. The `Author` information is the individual who originally created the patch and when it was originally created. 

But it’s possible that the patch won’t apply cleanly. Perhaps your main branch has diverged too far from the branch the patch was built from, or the patch depends on another patch you haven’t applied yet. In that case, the `git am` process will fail and ask you what you want to do:

	$ git am 0001-seeing-if-this-helps-the-gem.patch 
	Applying: seeing if this helps the gem
	error: patch failed: ticgit.gemspec:1
	error: ticgit.gemspec: patch does not apply
	Patch failed at 0001.
	When you have resolved this problem run "git am --resolved".
	If you would prefer to skip this patch, instead run "git am --skip".
	To restore the original branch and stop patching run "git am --abort".

This command puts conflict markers in any files it has issues with, much like a conflicted merge or rebase operation. You solve this issue much the same way — edit the file to resolve the conflict, stage the new file, and then run `git am --resolved` to continue to the next patch:

	$ (fix the file)
	$ git add ticgit.gemspec 
	$ git am --resolved
	Applying: seeing if this helps the gem

If you want Git to try a bit more intelligently to resolve the conflict, you can pass a `-3` option to it, which makes Git attempt a three-way merge. This option isn’t on by default because it doesn’t work if the commit the patch says it was based on isn’t in your repository. If you do have that commit — if the patch was based on a public commit — then the `-3` option is generally much smarter about applying a conflicting patch:

	$ git am -3 0001-seeing-if-this-helps-the-gem.patch 
	Applying: seeing if this helps the gem
	error: patch failed: ticgit.gemspec:1
	error: ticgit.gemspec: patch does not apply
	Using index info to reconstruct a base tree...
	Falling back to patching base and 3-way merge...
	No changes -- Patch already applied.

In this case, I was trying to apply a patch I had already applied. Without the `-3` option, it looks like a conflict.

If you’re applying a number of patches from an mbox, you can also run the `am` command in interactive mode, which stops at each patch it finds and asks if you want to apply it:

	$ git am -3 -i mbox
	Commit Body is:
	--------------------------
	seeing if this helps the gem
	--------------------------
	Apply? [y]es/[n]o/[e]dit/[v]iew patch/[a]ccept all 

This is nice if you have a number of patches saved, because you can view the patch first if you don’t remember what it is, or not apply the patch if you’ve already done so.

When all the patches for your topic are applied and committed into your branch, you can choose whether and how to integrate them into a longer-running branch.

### Checking Out Remote Branches ###

If your contribution came from a Git user who set up their own repository, pushed a number of changes into it, and then sent you the URL to the repository and the name of the remote branch the changes are in, you can add them as a remote and do merges locally.

For instance, if Jessica sends you an e-mail saying that she has a great new feature in the `ruby-client` branch of her repository, you can test it by adding the remote and checking out that branch locally:

	$ git remote add jessica git://github.com/jessica/myproject.git
	$ git fetch jessica
	$ git checkout -b rubyclient jessica/ruby-client

If she e-mails you again later with another branch containing another great feature, you can fetch and check out because you already have the remote setup.

This is most useful if you’re working with a person consistently. If someone only has a single patch to contribute once in a while, then accepting it over e-mail may be less time consuming than requiring everyone to run their own server and having to continually add and remove remotes to get a few patches. You’re also unlikely to want to have hundreds of remotes, each for someone who contributes only a patch or two. However, scripts and hosted services may make this easier — it depends largely on how you develop and how your contributors develop.

The other advantage of this approach is that you get the history of the commits as well. Although you may have legitimate merge issues, you know where in your history their work is based; a proper three-way merge is the default rather than having to supply a `-3` and hope the patch was generated off a public commit to which you have access.

If you aren’t working with a person consistently but still want to pull from them in this way, you can provide the URL of the remote repository to the `git pull` command. This does a one-time pull and doesn’t save the URL as a remote reference:

	$ git pull git://github.com/onetimeguy/project.git
	From git://github.com/onetimeguy/project
	 * branch            HEAD       -> FETCH_HEAD
	Merge made by recursive.

### Determining What Is Introduced ###

Now you have a topic branch that contains contributed work. At this point, you can determine what you’d like to do with it. This section revisits a couple of commands so you can see how you can use them to review exactly what you’ll be introducing if you merge this into your main branch.

It’s often helpful to get a review of all the commits that are in this branch but that aren’t in your master branch. You can exclude commits in the master branch by adding the `--not` option before the branch name. For example, if your contributor sends you two patches and you create a branch called `contrib` and applied those patches there, you can run this:

	$ git log contrib --not master
	commit 5b6235bd297351589efc4d73316f0a68d484f118
	Author: Scott Chacon <schacon@gmail.com>
	Date:   Fri Oct 24 09:53:59 2008 -0700

	    seeing if this helps the gem

	commit 7482e0d16d04bea79d0dba8988cc78df655f16a0
	Author: Scott Chacon <schacon@gmail.com>
	Date:   Mon Oct 22 19:38:36 2008 -0700

	    updated the gemspec to hopefully work better

To see what changes each commit introduces, remember that you can pass the `-p` option to `git log` and it will append the diff introduced to each commit.

To see a full diff of what would happen if you were to merge this topic branch with another branch, you may have to use a weird trick to get the correct results. You may think to run this:

	$ git diff master

This command gives you a diff, but it may be misleading. If your `master` branch has moved forward since you created the topic branch from it, then you’ll get seemingly strange results. This happens because Git directly compares the snapshots of the last commit of the topic branch you’re on and the snapshot of the last commit on the `master` branch. For example, if you’ve added a line in a file on the `master` branch, a direct comparison of the snapshots will look like the topic branch is going to remove that line.

If `master` is a direct ancestor of your topic branch, this isn’t a problem; but if the two histories have diverged, the diff will look like you’re adding all the new stuff in your topic branch and removing everything unique to the `master` branch.

What you really want to see are the changes added to the topic branch — the work you’ll introduce if you merge this branch with master. You do that by having Git compare the last commit on your topic branch with the first common ancestor it has with the master branch.

Technically, you can do that by explicitly figuring out the common ancestor and then running your diff on it:

	$ git merge-base contrib master
	36c7dba2c95e6bbb78dfa822519ecfec6e1ca649
	$ git diff 36c7db 

However, that isn’t convenient, so Git provides another shorthand for doing the same thing: the triple-dot syntax. In the context of the `diff` command, you can put three periods after another branch to do a `diff` between the last commit of the branch you’re on and its common ancestor with another branch:

	$ git diff master...contrib

This command shows you only the work your current topic branch has introduced since its common ancestor with master. That is a very useful syntax to remember.

### Integrating Contributed Work ###

When all the work in your topic branch is ready to be integrated into a more mainline branch, the question is how to do it. Furthermore, what overall workflow do you want to use to maintain your project? You have a number of choices, so I’ll cover a few of them.

#### Merging Workflows ####

One simple workflow merges your work into your `master` branch. In this scenario, you have a `master` branch that contains basically stable code. When you have work in a topic branch that you’ve done or that someone has contributed and you’ve verified, you merge it into your master branch, delete the topic branch, and then continue the process.  If we have a repository with work in two branches named `ruby_client` and `php_client` that looks like Figure 5-19 and merge `ruby_client` first and then `php_client` next, then your history will end up looking like Figure 5-20.

Insert 18333fig0519.png 
Figure 5-19. History with several topic branches.

Insert 18333fig0520.png
Figure 5-20. After a topic branch merge.

That is probably the simplest workflow, but it’s problematic if you’re dealing with larger repositories or projects.

If you have more developers or a larger project, you’ll probably want to use at least a two-phase merge cycle. In this scenario, you have two long-running branches, `master` and `develop`, in which you determine that `master` is updated only when a very stable release is cut and all new code is integrated into the `develop` branch. You regularly push both of these branches to the public repository. Each time you have a new topic branch to merge in (Figure 5-21), you merge it into `develop` (Figure 5-22); then, when you tag a release, you fast-forward `master` to wherever the now-stable `develop` branch is (Figure 5-23).

Insert 18333fig0521.png 
Figure 5-21. Before a topic branch merge.

Insert 18333fig0522.png 
Figure 5-22. After a topic branch merge.

Insert 18333fig0523.png 
Figure 5-23. After a topic branch release.

This way, when people clone your project’s repository, they can either check out master to build the latest stable version and keep up to date on that easily, or they can check out develop, which is the more cutting-edge stuff.
You can also continue this concept, having an integrate branch where all the work is merged together. Then, when the codebase on that branch is stable and passes tests, you merge it into a develop branch; and when that has proven itself stable for a while, you fast-forward your master branch.

#### Large-Merging Workflows ####

The Git project has four long-running branches: `master`, `next`, and `pu` (proposed updates) for new work, and `maint` for maintenance backports. When new work is introduced by contributors, it’s collected into topic branches in the maintainer’s repository in a manner similar to what I’ve described (see Figure 5-24). At this point, the topics are evaluated to determine whether they’re safe and ready for consumption or whether they need more work. If they’re safe, they’re merged into `next`, and that branch is pushed up so everyone can try the topics integrated together.

Insert 18333fig0524.png 
Figure 5-24. Managing a complex series of parallel contributed topic branches.

If the topics still need work, they’re merged into `pu` instead. When it’s determined that they’re totally stable, the topics are re-merged into `master` and are then rebuilt from the topics that were in `next` but didn’t yet graduate to `master`. This means `master` almost always moves forward, `next` is rebased occasionally, and `pu` is rebased even more often (see Figure 5-25).

Insert 18333fig0525.png 
Figure 5-25. Merging contributed topic branches into long-term integration branches.

When a topic branch has finally been merged into `master`, it’s removed from the repository. The Git project also has a `maint` branch that is forked off from the last release to provide backported patches in case a maintenance release is required. Thus, when you clone the Git repository, you have four branches that you can check out to evaluate the project in different stages of development, depending on how cutting edge you want to be or how you want to contribute; and the maintainer has a structured workflow to help them vet new contributions.

#### Rebasing and Cherry Picking Workflows ####

Other maintainers prefer to rebase or cherry-pick contributed work on top of their master branch, rather than merging it in, to keep a mostly linear history. When you have work in a topic branch and have determined that you want to integrate it, you move to that branch and run the rebase command to rebuild the changes on top of your current master (or `develop`, and so on) branch. If that works well, you can fast-forward your `master` branch, and you’ll end up with a linear project history.

The other way to move introduced work from one branch to another is to cherry-pick it. A cherry-pick in Git is like a rebase for a single commit. It takes the patch that was introduced in a commit and tries to reapply it on the branch you’re currently on. This is useful if you have a number of commits on a topic branch and you want to integrate only one of them, or if you only have one commit on a topic branch and you’d prefer to cherry-pick it rather than run rebase. For example, suppose you have a project that looks like Figure 5-26.

Insert 18333fig0526.png 
Figure 5-26. Example history before a cherry pick.

If you want to pull commit `e43a6` into your master branch, you can run

	$ git cherry-pick e43a6fd3e94888d76779ad79fb568ed180e5fcdf
	Finished one cherry-pick.
	[master]: created a0a41a9: "More friendly message when locking the index fails."
	 3 files changed, 17 insertions(+), 3 deletions(-)

This pulls the same change introduced in `e43a6`, but you get a new commit SHA-1 value, because the date applied is different. Now your history looks like Figure 5-27.

Insert 18333fig0527.png 
Figure 5-27. History after cherry-picking a commit on a topic branch.

Now you can remove your topic branch and drop the commits you didn’t want to pull in.

### Tagging Your Releases ###

When you’ve decided to cut a release, you’ll probably want to drop a tag so you can re-create that release at any point going forward. You can create a new tag as I discussed in Chapter 2. If you decide to sign the tag as the maintainer, the tagging may look something like this:

	$ git tag -s v1.5 -m 'my signed 1.5 tag'
	You need a passphrase to unlock the secret key for
	user: "Scott Chacon <schacon@gmail.com>"
	1024-bit DSA key, ID F721C45A, created 2009-02-09

If you do sign your tags, you may have the problem of distributing the public PGP key used to sign your tags. The maintainer of the Git project has solved this issue by including their public key as a blob in the repository and then adding a tag that points directly to that content. To do this, you can figure out which key you want by running `gpg --list-keys`:

	$ gpg --list-keys
	/Users/schacon/.gnupg/pubring.gpg
	---------------------------------
	pub   1024D/F721C45A 2009-02-09 [expires: 2010-02-09]
	uid                  Scott Chacon <schacon@gmail.com>
	sub   2048g/45D02282 2009-02-09 [expires: 2010-02-09]

Then, you can directly import the key into the Git database by exporting it and piping that through `git hash-object`, which writes a new blob with those contents into Git and gives you back the SHA-1 of the blob:

	$ gpg -a --export F721C45A | git hash-object -w --stdin
	659ef797d181633c87ec71ac3f9ba29fe5775b92

Now that you have the contents of your key in Git, you can create a tag that points directly to it by specifying the new SHA-1 value that the `hash-object` command gave you:

	$ git tag -a maintainer-pgp-pub 659ef797d181633c87ec71ac3f9ba29fe5775b92

If you run `git push --tags`, the `maintainer-pgp-pub` tag will be shared with everyone. If anyone wants to verify a tag, they can directly import your PGP key by pulling the blob directly out of the database and importing it into GPG:

	$ git show maintainer-pgp-pub | gpg --import

They can use that key to verify all your signed tags. Also, if you include instructions in the tag message, running `git show <tag>` will let you give the end user more specific instructions about tag verification.

### Generating a Build Number ###

Because Git doesn’t have monotonically increasing numbers like 'v123' or the equivalent to go with each commit, if you want to have a human-readable name to go with a commit, you can run `git describe` on that commit. Git gives you the name of the nearest tag with the number of commits on top of that tag and a partial SHA-1 value of the commit you’re describing:

	$ git describe master
	v1.6.2-rc1-20-g8c5b85c

This way, you can export a snapshot or build and name it something understandable to people. In fact, if you build Git from source code cloned from the Git repository, `git --version` gives you something that looks like this. If you’re describing a commit that you have directly tagged, it gives you the tag name.

The `git describe` command favors annotated tags (tags created with the `-a` or `-s` flag), so release tags should be created this way if you’re using `git describe`, to ensure the commit is named properly when described. You can also use this string as the target of a checkout or show command, although it relies on the abbreviated SHA-1 value at the end, so it may not be valid forever. For instance, the Linux kernel recently jumped from 8 to 10 characters to ensure SHA-1 object uniqueness, so older `git describe` output names were invalidated.

### Preparing a Release ###

Now you want to release a build. One of the things you’ll want to do is create an archive of the latest snapshot of your code for those poor souls who don’t use Git. The command to do this is `git archive`:

	$ git archive master --prefix='project/' | gzip > `git describe master`.tar.gz
	$ ls *.tar.gz
	v1.6.2-rc1-20-g8c5b85c.tar.gz

If someone opens that tarball, they get the latest snapshot of your project under a project directory. You can also create a zip archive in much the same way, but by passing the `--format=zip` option to `git archive`:

	$ git archive master --prefix='project/' --format=zip > `git describe master`.zip

You now have a nice tarball and a zip archive of your project release that you can upload to your website or e-mail to people.

### The Shortlog ###

It’s time to e-mail your mailing list of people who want to know what’s happening in your project. A nice way of quickly getting a sort of changelog of what has been added to your project since your last release or e-mail is to use the `git shortlog` command. It summarizes all the commits in the range you give it; for example, the following gives you a summary of all the commits since your last release, if your last release was named v1.0.1:

	$ git shortlog --no-merges master --not v1.0.1
	Chris Wanstrath (8):
	      Add support for annotated tags to Grit::Tag
	      Add packed-refs annotated tag support.
	      Add Grit::Commit#to_patch
	      Update version and History.txt
	      Remove stray `puts`
	      Make ls_tree ignore nils

	Tom Preston-Werner (4):
	      fix dates in history
	      dynamic version method
	      Version bump to 1.0.2
	      Regenerated gemspec for version 1.0.2

You get a clean summary of all the commits since v1.0.1, grouped by author, that you can e-mail to your list.

## Summary ##

You should feel fairly comfortable contributing to a project in Git as well as maintaining your own project or integrating other users’ contributions. Congratulations on being an effective Git developer! In the next chapter, you’ll learn more powerful tools and tips for dealing with complex situations, which will truly make you a Git master.
