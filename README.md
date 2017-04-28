# LanyokNapja17
A short introduction into web developement

Az alábbiakban Dmitri Sotnikov [Web Development with Clojure](https://pragprog.com/book/dswdcloj2/web-development-with-clojure-second-edition) könyve első fejezetének megfelelően haladunk.

# Előkészítés
Ahogy a könyv címe is jelzi, a [Clojure](http://www.clojure.com/) nyelvet fogjuk használni a továbbiakban.
A szerver oldali programozásra több száz programozási nyelv közül is választhatnánk, ebben az esetben is van egy keretrendszer, ami nagyban elősegíti, és felgyorsítja a munkát, hasonlóan a [Ruby on Rails](http://rubyonrails.org/) vagy például a [Yii](http://www.yiiframework.com/) rendszerekhez; viszont érzésem szerint itt kisebb gondot jelent a nyelvvel ismerkedés.

Természetesen mi is óriások vállán állunk, kész programkönyvtárakat használunk. Ezek összegyűjtésében, telepítésében segítséget nyújt egy aprócska kis program, a [Leinigen](https://leiningen.org/). Windows alá ezt legegyszerűbben a [https://djpowell.github.io/leiningen-win-installer/](https://djpowell.github.io/leiningen-win-installer/) oldalról telepíthetjük, akár rendszergazdai jogosultságok nélkül is. 

A telepítés lezajlik pár perc alatt. Alapértelmezetten megkapunk egy terminált, ahol egyből ki is próbálhatjuk, hogy minden rendben működik:

    (+ 1 2 3 4 5)
    
Az alábbiakat begépelve ```15``` eredményt kell kapnunk, mert az ```1+2+3+4+5``` értékét számítottuk ki. A Clojure a Lisp nyelvek családjába tartozik, ahol az ```f(x,y)``` függvényhívást ```(f x y)``` módon kell leírnunk.

Ha minden rendben ment, akkor Ctrl+D segítségével ki is léphetünk, mert más módon használjuk ezt a programot.

# Vendégkönyv 
A programírás folyamatát egy vendégkönyv elkészítésén mutatjuk meg. Itt a látogató üzeneteket hagyhat az oldal tulajdonosának. 

Az egyes weblapok végén megszokott kommentek esetén fontos a kommentek összekapcsolódása, ki kinek a megjegyzésére válaszolt, ezért az bonyolultabb belső struktúrát feltételez. A vendégkönyv esetén csak egymást követik az egyes bejegyzések, nincs köztük kapcsolat. Egy-egy bejegyzés magából az üzenetből, a bejegyzés írójának nevéből, és egy dátumból fog állni. 

## Sablon használata
A Leinigen több fajta sablont is ismer, ezek közül az adott feladathoz leginkább illőt kell használni. A sablonok lényege, hogy kilakul egy szabványos alkönyvtárrendszer, és így az adott fejlesztésben részt vevők bármelyike egyből megtalál mindent, nem kell keresgetni az egyes állományokat. Miután mi egy webes progamot készítenénk, a következő utatítást kell kiadnunk parancssorból (```cmd```):

    lein new compojure-app guestbook
    
A _lein_ program egyik nagy hátránya, hogy lassan indul el. Viszont szerencsére ritkán kell elindítani egy hagyományos fejlesztés során.  
A _guestbook_ a vendégkönyv angol megfelelője. Miután a programfejlesztés nemzetközivé vált, ha egy magyar elnevezésekkel (változókkal, függvénynevekkel, stb.) megírt program továbbfejlesztésébe vonnánk be kínai, indiai vagy éppen orosz fejlesztőket, akkor első dolgunk az lehetne, hogy már a kész program szövegét egy közös nyelvre kellene lefordítani. Ha már angolul készül el az első verzió is, ezt a pluszmunkát elkerülhetjük. Lássuk mit csináltunk eddig! Lépjünk be a legenerált alkönyvtárba (```cd guestbook```), és adjuk ki az alábbi parancsot:

    lein ring server


Ezzel elindult a gépünkön egy webszerver, és az elinduló böngészőnkben már láthatjuk is a végeredményt, ami természetesen elég soványka. De mit várnánk el egy sablontól?

Mindkét parancs indítása után a _lein_ letölti azokat a kisebb-nagyobb programkönyvtárakat, melyekre a webszerver futásához szükség van. Természetesen egy programkönyvtárat csak egyszer tölt le, a továbbiakban ezek már megtalálhatóak a gépünkön. 

## Az oldal átírása

A sablon nyújtotta lehetőségeken túl kell lépni, hogy elérjük a célunkat. 
Nem árt pár szót ejteni a generált alkönyvtárrendszerről. A _resources_ könyvtárba kerül az oldalak kinézetét befolyásoló minden fájl: a megjelenítendő képek, az oldalstílusokat meghatározó CSS állományok, illetve a kliens oldalon futó Javascript programocskák. Ha az elkészült programunkat valahol üzembe helyeznénk, és ehhez lefordítanánk és összecsomagolnánk, az a _target_ könyvtárba kerülne. Az _src_ könyvtárban található maga a forrás, amit hamarosan kiegészítünk, míg a _test_ alkönyvtárba kerülnek azok a különféle tesztek, melyekkel az elkészült programunk egyes részeit tesztelhetjük, megbizonyosodva, hogy egyes továbbfejlesztések nem rontottak-e el valami korábban még működőképes részt.

Lépjünk be a ```src/guestbook/routes``` könyvtárba. Itt található meg az egyes webcímekhez hozzákapcsolt funkcionalitás. Azaz nevezetesen ha csak a webszerverünk címét írnánk be (ami most ```localhos:3000```), akkor szeretnénk az üzeneteket látni. Mindez a ```home.clj``` fájlban van definiálva.  A fájl első három sora a felhasznált programkönyvtárakat adja meg, míg az utolsó kettő az előbb említett hozzárendelést adja meg. Nevezetesen, ha semmi nem szerepel az előbbi cím után, akkor a ```home``` függvényt kell végrehajtani. 

    (defn home []
      (layout/common 
        [:h1 "Guestbook"]
        [:p "Welcome to my guestbook"]
        [:hr]
        [:form
          [:p "Name"]
          [:input]
          [:p "Message"]
          [:textarea {:rows 10 :cols 40}]]))

# Próbálkozás
A mintafájlok a [https://arato.inf.unideb.hu/aszalos.laszlo/lanyok.html](https://arato.inf.unideb.hu/aszalos.laszlo/lanyok.html)
oldalon találhatóak.
