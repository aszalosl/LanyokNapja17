# LanyokNapja17
A short introduction into web developement

_Ez az oldal a DE IK 2017-es Lányok Napján megtartott megtartott előadás írott változata._

Az alábbiakban Dmitri Sotnikov [Web Development with Clojure](https://pragprog.com/book/dswdcloj2/web-development-with-clojure-second-edition) könyve első fejezetének megfelelően haladunk.

# Előkészítés
Ahogy a könyv címe is jelzi, a [Clojure](http://www.clojure.com/) nyelvet fogjuk használni a továbbiakban.
A szerver oldali programozásra több száz programozási nyelv közül is választhatnánk, ebben az esetben is van egy keretrendszer, ami nagyban elősegíti, és felgyorsítja a munkát, hasonlóan a [Ruby on Rails](http://rubyonrails.org/) vagy például a [Yii](http://www.yiiframework.com/) rendszerekhez; viszont érzésem szerint itt kisebb gondot jelent a nyelvvel ismerkedés.

Természetesen mi is óriások vállán állunk, kész programkönyvtárakat használunk. Ezek összegyűjtésében, telepítésében segítséget nyújt egy aprócska kis program, a [Leinigen](https://leiningen.org/). Windows alá ezt legegyszerűbben a [https://djpowell.github.io/leiningen-win-installer/](https://djpowell.github.io/leiningen-win-installer/) oldalról telepíthetjük, akár rendszergazdai jogosultságok nélkül is. 

A telepítés lezajlik pár perc alatt. Alapértelmezetten megkapunk egy terminált, ahol egyből ki is próbálhatjuk, hogy minden rendben működik-e:

    (+ 1 2 3 4 5)
    
Az alábbiakat begépelve ```15``` eredményt kell kapnunk, mert az ```1+2+3+4+5``` értékét számítottuk ki. A Clojure a Lisp nyelvek családjába tartozik, ahol az ```f(x,y)``` függvényhívást ```(f x y)``` módon kell leírnunk.

Ha minden rendben ment, akkor Ctrl+D segítségével ki is léphetünk, mert más módon fogjuk használni ezt a programot.

# Üzenőfal
A programírás folyamatát egy üzenőfal/vendégkönyv elkészítésén mutatjuk meg. Itt a látogató üzeneteket hagyhat az oldal tulajdonosának. 

Az egyes weblapok végén megszokott kommentek esetén fontos a kommentek összekapcsolódása, ki kinek a megjegyzésére válaszolt, ezért az bonyolultabb belső struktúrát feltételez. A vendégkönyv esetén csak egymást követik az egyes bejegyzések, nincs köztük kapcsolat. Egy-egy bejegyzés magából az üzenetből, a bejegyzés írójának nevéből, és egy dátumból fog állni. 

## Sablon használata
A Leinigen több fajta sablont is ismer, ezek közül az adott feladathoz leginkább illőt kell használni. A sablonok lényege, hogy kialakul egy szabványos alkönyvtárrendszer, és így az adott fejlesztésben részt vevők bármelyike egyből megtalál mindent, nem kell keresgetni az egyes állományokat. Miután mi egy webes programot készítenénk, a következő utatítást kell kiadnunk parancssorból (```cmd```):

    lein new compojure-app guestbook
    
A _lein_ program egyik nagy hátránya, hogy lassan indul el. Viszont szerencsére ritkán kell elindítani egy hagyományos fejlesztés során.  
A _guestbook_ a vendégkönyv angol megfelelője. Miután a programfejlesztés nemzetközivé vált, ha egy magyar elnevezésekkel (változókkal, függvénynevekkel, stb.) megírt program továbbfejlesztésébe vonnánk be kínai, indiai vagy éppen orosz fejlesztőket, akkor első dolgunk az lehetne, hogy már a kész program szövegét egy közös nyelvre kellene lefordítani. Ha már angolul készül el az első verzió is, ezt a pluszmunkát elkerülhetjük. Lássuk mit csináltunk eddig! Lépjünk be a generált alkönyvtárba (```cd guestbook```), és adjuk ki az alábbi parancsot:

    lein ring server

Ezzel elindult a gépünkön egy webszerver, és az elinduló böngészőnkben már láthatjuk is a végeredményt, ami természetesen elég soványka. De mit várnánk el egy sablontól?

Mindkét parancs indítása után a _lein_ letölti azokat a kisebb-nagyobb programkönyvtárakat, melyekre a webszerver futásához szükség van. Természetesen egy programkönyvtárat csak egyszer tölt le, a továbbiakban ezek már megtalálhatóak a gépünkön. 

## Az oldal átírása

A sablon nyújtotta lehetőségeken túl kell lépni, hogy elérjük a célunkat. 
Nem árt pár szót ejteni a generált alkönyvtárrendszerről. 
* A _resources_ könyvtárba kerül az oldalak kinézetét befolyásoló minden fájl: a megjelenítendő képek, az oldalstílusokat meghatározó CSS állományok, illetve a kliens oldalon futó Javascript programocskák. 
* Ha az elkészült programunkat valahol üzembe helyeznénk, és ehhez lefordítanánk és összecsomagolnánk, az a _target_ könyvtárba kerülne. 
* Az _src_ könyvtárban található maga a forrás, amit hamarosan kiegészítünk. 
* Végül a _test_ alkönyvtárba kerülnek azok a különféle tesztek, melyekkel az elkészült programunk egyes részeit tesztelhetjük, megbizonyosodva, hogy egyes továbbfejlesztések nem rontottak-e el valami korábban még működőképes részt.

Lépjünk be a ```src/guestbook/routes``` könyvtárba. Itt található meg az egyes webcímekhez kapcsolt funkcionalitás. Azaz nevezetesen ha csak a webszerverünk címét írnánk be (ami most ```localhos:3000```), akkor szeretnénk az üzeneteket látni. Mindez a ```home.clj``` fájlban van definiálva.  A fájl első három sora a felhasznált programkönyvtárakat adja meg, míg az utolsó kettő az előbb említett hozzárendelést adja meg. Nevezetesen, ha semmi nem szerepel az előbbi cím után, akkor a ```home``` függvényt kell végrehajtani. A korábbi kétsoros alakot cseréljük le az alábbira:


    (defn home []
      (layout/common 
        [:h1 "Üzenőfal"]
        [:p "Üdvözlöm az üzenőfalamon"]
        [:hr]
        [:form
          [:p "Név"]
          [:input]
          [:p "Üzenet"]
          [:textarea {:rows 10 :cols 40}]]))

Ha valaki tanult egy kis webszerkesztést, akkor felismer pár kulcsszót (h1, p, form, input, stb) az alábbi részben, bár itt nem kacsacsőrök között szerepel, mint egy HTML forrásban, sőt még lezáró tagok sem találhatóak. A Hiccup könyvtár lehetővé teszi az előbb látható speciális nyelv használatát, ami véleményem szerint sokkal kényelmesebb, mint egy sablonrendszer használata (mint például a [Smarty](http://www.smarty.net/), a [mustache](https://mustache.github.io/), a [Jade](https://www.npmjs.com/package/jade) vagy a [Jinja](http://jinja.pocoo.org/), hogy csak párat említsek a több száz közül).

Frissítve a böngészőkben az oldalunkat (Ring webszerver újraindítása nélkül) már az előbbi függvénynek megfelelően jelenik meg az oldal. Ha valaki gondolja, az angol nyelvű szöveget nyugodtan lecserélheti magyarra, vagy a bekezdés helyett alcímet készíthet az üdvözlő szövegből.

## Az űrlap továbbfejlesztése

Az oldalon a vonal alatt található űrlapba írhatunk, csak elküldeni nincs lehetőségünk, hiányzik a megfelelő nyomógomb. Ezért egy fokkal jobban kezelhető programcsomagot (hiccup.form) veszünk igénybe. Viszont emiatt a ```home.clj``` első három sorát le kell cserélnünk az alábbira:

    (ns guestbook.routes.home
      (:require [compojure.core :refer :all]
                [guestbook.views.layout :as layout]
                [hiccup.form :refer :all]))

Annak érdekében, hogy tesztelhessük az oldalunkat, elhelyezünk pár bejegyzést. Az alábbi függvény ezeket a bejegyzéseket tartalmazza, illetve ezek megjelenítésének módját.

    (defn show-guests []
      [:ul.guests
        (for [{:keys [message name timestamp]}
               [{:message Szia" :name "Szabi" :timestamp nil}
                {:message "Hello" :name "Hugó" :timestamp nil}]]
          [:li
            [:blockquote message] [:p "-" [:cite name]] [:time timestamp]])])

Az ```ul``` egy felsorolásra utal, a mögötte álló ```guests``` egy osztályt definiál, melyet majd a grafikai stílusok megadásánál lehet felhasználni, hogy más kinézete legyen ennek a résznek, mint a honlap többi részének.

Az itt található _for_ ciklus igencsak eltér a korábban láthatóaktól. Végigfut a két mintabejegyzésen, és a legutolsó sor szerinti formázással jeleníti meg azokat egy felsorolás belsejében.

Annak érdekében, hogy az előbbi függvényt használatba vehessük, valamint, hogy kihasználhassuk az űrlapok fejlettebb formázási lehetőségeit, át kell írnunk a ```home``` függvényt:

    (defn home [& [name message error]]
      (layout/common 
        [:h1 "Üzenőfal"]
        [:p "Üdvözlöm az üzenőfalamon!"]
        [:p error]
        (show-guests)
        [:hr]
        (form-to [:post "/"]
          [:p "Név"] (text-field "name" name)
          [:p "Üzenet"] (text-area {:rows 10 :cols 40} "message" message)
          [:br]
          (submit-button "comment"))))

Az űrlap definíciójában szerepelt, hogy ez is a _localhost:3000_ címen kerül feldolgozásra, csupán egy más hozzáféréssel. Ennek megfelelően a fájl utolsó két sorát frissíteni kell, megadva a feldolgozó függvényt:

    (defroutes home-routes
      (GET "/" [] (home))
      (POST "/" [name message] (save-message name message)))

Ez a feldolgozó függvény még hiányzik, szúrjuk be a _home_ függvény és az előbbi hozzárendelések közé!

    (defn save-message [name message]
      (cond
        (empty? name) (home name message "Valaki nem adta meg a nevét!")
        (empty? message) (home name message "Mit szerettél volna üzenni?")
        :else
          (do
            (println name message)
            (home))))

Ez a függvény alapjában véve egy elágazást tartalmaz, ebben az első két feltétel azt ellenőrni, hogy nem maradt-e valami üresen az űrlapban, és ha igen, akkor ezzal az információval, valamint a megadott adatokkal jeleníti meg az oldalunkat. Ez lehetőséget ad arra, hogy a felhasználó megadhassa a hiányzó adatot. Ha nincs hiányzó adat, akkor pedig kiírja az űrlapon megadott nevet és üzenetet, s majd meghívja az oldalunkat megjelenítő függvényt.

Ha kipróbáljuk a új weboldalunkat, akkor a hibaüzenetek látszódnak, viszont a megadott adatok nem jelennek meg. Nem is csoda, mert a ```println``` nem a böngészőben, hanem a konzolra ír. 

A felhasználó által megadott adatok nem kerülnek be a mintaként megadott üzenetek közé. Ahhoz, hogy ezt elérjük, szintet kell lépnünk, s üzembe kell helyeznünk egy adatbázist, mely majd az üzeneteket fogja tárolni.

## Adatbázis hozzáadása

A korábbiakhoz képest további könyvtárakra is szükség van az adatbázisunk használatához. Ehhez a külső alkönyvtárban található ```project.cls``` fájlt kell módosítani. Lényegében az alsó két sort kell beszúrnunk:

    :dependencies [[org.clojure/clojure "1.8.0"]
                   [compojure "1.5.2"]
                   [hiccup "1.0.5"]
                   [ring-server "0.4.0"]
                   ;; adatbázis
                   [org.clojure/java.jdbc "0.2.3"]
                   [org.xerial/sqlite-jdbc "3.7.2"]]
                   
Ezt követően elkezdhetjük az adatbázisunkat kezelő függvényeket írni. Ezek a kvázi szabványnak megfelelően az ```src/guestbook/models``` alkönyvtárban kapnak helyet, mondjuk egy ```db.clj``` fájlban.

Először is fel kell használnunk az előzőleg megadott könyvtárakat, majd meg kell adnunk azt a fájlt (```db.sq3```), mely az adatbázist fogja tartalmazni. Erre a fájlra a későbbiekben ```db``` néven hivatkozunk.

    (ns guestbook.models.db
      (:require [clojure.java.jdbc :as sql])
      (:import java.sql.DriverManager))

    (def db {:classname "org.sqlite.JDBC",
             :subprotocol "sqlite",
             :subname "db.sq3"})

Ahelyett, hogy valamilyen adatbáziskezelő programban összekattintgassuk az új adatbázisunkat, inkább egy függvénnyel hozzuk létre. Egy ilyen egyszerű adattáblánál ennek az előnye nem látszik, de ha sikeres lesz a program, és rengeteg helyen kell telepíteni, valamint az adatbázis adattáblák tucatjait tartalmazza, akkor ez az út jár kevesebb bonyodalommal. Nem megyünk bele az SQL nyelv rejtelmeibe, most elég az tudni, hogy egy bejegyzésazonosítót, egy nevet, egy üzenetet és egy időpontot tárolunk le minden egyes bejegyzésnél:

    (defn create-guestbook-table []
      (sql/with-connection
        db
        (sql/create-table
          :guestbook
          [:id "INTEGER PRIMARY KEY AUTOINCREMENT"]
          [:timestamp "TIMESTAMP DEFAULT CURRENT_TIMESTAMP"]
          [:name "TEXT"]
          [:message "TEXT"])
        (sql/do-commands "CREATE INDEX timestamp_index ON guestbook (timestamp)")))

Természetesen azért van adatbázisunk, hogy a tartalmát megjeleníthessük az oldalunkra odatévedőnek. Így kell egy függvény, mellyel kiolvashatjuk az adatbázis tartalmát. Aki az SQL-ben járatos, láthatja, hogy a bejegyzéseket a koruk szerint fogja sorbaállítani:

    (defn read-guests []
      (sql/with-connection
        db
        (sql/with-query-results res
          ["SELECT * FROM guestbook ORDER BY timestamp DESC"]
          (doall res))))
          
Természetesen az adatbázisból csak akkor nyerhetünk ki bármit is, ha oda raktunk is valamit, így kell egy függvény az adataink elmentéséhez is. A függvény csak két adatot vár a négyből, a másik kettő automatikusan kap értéket. Itt lehet látni, hogy szükség esetén a Clojure az alapjául szolgáló Java adottságait fel tudja használni:

    defn save-message [name message]
      (sql/with-connection
        db
        (sql/insert-values
          :guestbook
          [:name :message :timestamp]
          [name message (new java.util.Date)])))

Miután elmentettük a fájlunkat, vegyük használatba és hozzuk létre az adatbázist! Ehhez egy új terminált kell kinyitnunk a külső könyvtárban (ahol a ```project.clj``` is található), s ki kell adni a ```lein repl``` parancsot. Ezzel elindul a repl, ahol két utasításra lesz szükségünk. Az első betölti az előbb megírt fájlt, míg a második már innen futtatja az adatbázis generálást:

    (use 'guestbook.models.db)
    (create-guestbook-table)

Ellenőrizhetjük, hogy valóban elkészült az adatbázisunk, és mehetünk tovább. (Aki ott volt a bemutatón, talán emlékszik, hogy az első sorból kimaradt az utolsó _s_ betű, s ez már elég, hogy ne lehessen kiadni a második utasítást.)

## Adatbázis beüzemelése

Térjünk vissza újra az ```src/guestbook/routes/home.clj``` fájlunkhoz! Szükségünk van az előbb megírt fájra, így hivatkozni kell rá a fájl elején (csak az utolsó sor módosítottuk):

    (ns guestbook.routes.home
      (:require [compojure.core :refer :all]
                [guestbook.views.layout :as layout]
                [hiccup.form :refer :all]
                [guestbook.models.db :as db]))

A ```show-guests``` függvény csak előre megadott mintaadatokat mutatott újra és újra. Itt az idő, hogy használja az adatbázist, így cseréljük le az alábbira:

    (defn show-guests []
      [:ul.guests
        (for [{:keys [message name timestamp]} (db/read-guests)]
          [:li
            [:blockquote message]
            [:p "-" [:cite name]]
            [:time timestamp]])])

Ha valaki szeretné, további formázással is kiegészíthetné ezt a felsorolást, bár inkább az oldalt megjelenítő CSS stílusok segítségével érdemes módosítani a kinézetét. (A dizájn módosítása már rendszerint egy más munkatárs feladata, így nem megyünk bele.)

Feladatunk még a begépelt üzenetek mentése. Ehhez lényegében a korábbi ```println``` utasítást kell lecserélni a megfelelő függvény nevére (```db/save-message```) és már kész is működő üzenőfalunk:

    (defn save-message [name message]
      (cond
        (empty? name)     (home name message "Some dummy forgot to leave a name")
        (empty? message)  (home name message "Don't you have something to say?")
        :else             (do
                            (db/save-message name message)
                            (home))))
                           
Egy kis gyakorlattal ez az egyszerű programocska közel félóra alatt elkészül. Még rá lehet fordítani öt percet, hogy a dátum olvasható formában jelenjen meg. S a saját gépünkről áttelepíthetjük egy valódi, bárhonnan elérhető szerverre.

Egy nagyobb webes rendszer rendszerint ilyen kis programocskák tucatjait, százait tartalmazza, egy komplexebb adatbázist használva. Természetesen figyelni kell, hogy csak az arra [jogosultak](https://en.wikipedia.org/wiki/Authentication) érhessék el a rendszer egyes szolgáltatásait, és hogy az űrlapokba beírt adatok megfelelőek legyenek.

# Olvasnivaló
## Magyarul
* [Fejlesztők bemutatkozása](http://egyperctech.reblog.hu/)
* [ELTE Web-fejlesztés](http://webfejlesztes.inf.elte.hu/)
* [Nagy Gusztáv könyve](http://web.progtanulo.hu/)
* [ELTE-s magasabb szintű tárgyak](http://webprogramozas.inf.elte.hu)
## Angolul
* [Smashing Magazine](https://www.smashingmagazine.com) 
