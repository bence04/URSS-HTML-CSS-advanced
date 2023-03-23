# SCSS-Basics

### Mi az a SASS (Syntactically Awesome Style Sheets)?

Egy olyan script-nyelv, ami  a fordítás során képes CSS-é alakulni . Úgy is nevezik, hogy **CSS Pre-processor**.

A honlapok stílus jegyeinek formázása során igény támadt arra, hogy a programozási (script)
nyelvekhez hasonlóan lehetőség legyen változók deklarálására, függvények és alapműveletek végrehajtására.
Magyarul a stílus-jegyek "forráskódjait" gazdaságosabban használhassuk fel.
Az ismétlések, túl hosszú kódok elkerülése és a kódrészletek újrahasznosítására lett a cél.


SASS vagy SCSS?

Igazából bármelyikről is beszélünk, szinte teljesen ugyan arról az eredetről beszélünk.

- SASS: öregebb szintaktika. (például nem kapcsos zárójeleket használ, mint a CSS). Kiterjesztése: .sass
- SCSS: a CSS "szuper gyűjteménye", minden CSS szabály érvényes rá. Alapvetően ezt használjuk. Kiterjesztése: .scss
Mindkettőre igaz, hogy szükségünk van a sass fordítóra (sass pre-compiler), aminek a használata során SCSS file lefordítódik és létrehozza megfelelő CSS file-t.



### Hogyan tudjuk telepíteni az eszközt?

Többféleképen tudjuk telepíteni a **sass compiler**-t.

Lásd. => [SASS TELEPÍTÉSI LEHETŐSÉGEK](https://sass-lang.com/install)

Talán a legegyszerübb globális telepítés a node package manager-rel történhet:

```npm install -g sass```

Amennyiben még nem telepítettük a Node-t a gépünkre => [itt a lehetőség hozzá](https://nodejs.org/en/download/)


### A SCSS file lefordítása CSS file-ra:

A böngésző **NEM tudja futtatni** a SCSS file-t, ezért kell lefordítani a SCSS-t CSS fájlra.
Az SCSS elsősorban a fejlesztés támogatására szolgál.
Két lépéssel tudunk lefordítani egy scss kiterjesztésű fájlt:

1. terminalban **'sass'** parancs +  megadjuk a relative elérési utját a forrás scss file-nak.
2. megadjuk a célt a relatív elérési úttal, hogy melyik file-ba fordítsa le.

Gyors példa:

 ```sass home/scss-projekt/scss/stlye.scss home/scss-projekt/css/style.css```

Van lehetőségünk ***folyamatos*** állapot frissítésre a ```--watch``` flag használatával:

```sass --watch home/scss-projekt/scss/stlye.scss home/scss-projekt/css/style.css ```

 Ekkor minden frissítés után automatikusan lefordítja a CSS file-t


**Intellij SASS Plugin:**

Van lehetőség az Intellij IDEA Ultimate-hez beépített plugint használni, ami egyszerűsítheti a SASS kód lefordítását.

Két plugin-nal kell ehhez rendelkeznünk az IDEA-ban
1. **SASS**
2. **File Watcher**

Az említett pluginok használatáról részletesen itt a [JETBRAINS dokumentáció](https://www.jetbrains.com/help/idea/transpiling-sass-less-and-scss-to-css.html#ws_sass_less_scss_syntax_highlighting)

 **Egy klasszikus statikus weboldal file-struktúra példa, ha scss pre-processor-t használunk**:

```
dist                             // a lefordított css tartalom
    |- style
    |   |-style1.css
    |   |-style2.css
    |
    |- index.html

src
   |- scss -
       |   |-module                 // modulok, vagyis az előre megírt és újrahasznosításra váró kódrészletek
       |        |
       |        |- _mixins.scss
       |        |- _colors.scss
       |
       |- style1.scss               // az scss file-ok, melyek felhasználhatnak funkciókat a modulokból
       |- style2.scss
```


### Milyen fő elemekkel és funkciókkal egészíti ki a CSS-t a pre-processor?

- kódrészletek egymásba ágyazása (Nesting)
- változók deklarálása
- CSS szabályok újrahasznosítása (Mixin)
- export-import műveletek
- függvények
- ciklusok

Mi most az első 4 elemre fogunk fókuszálni.

## A Nesting

A kód részletek (CSS-szabályok) egymásba ágyazásának kiemelt szerepe van.
Segítségével rövidülnek a sorok számai, és egyértelműen csoportosulnak az összetartozó css-szabályok.
Nézzük meg ezt egy kis kódrészletben:

**index.html**:

```html
<nav>
    <a href="">Link</a>
</nav>
```

Ha összetett szelektorokat szeretnénk használni egy css file-ban akkor valahogy így nézne ki a file-junk:


**style.css**:

```css
nav a {
   color: purple;
   display: inline-block;
}
```

Most ugyanezt egy scss file-ban létrehozva:

**style.scss**:

```scss
nav {
       a {
        color: purple;
        display: inline-block;
     }
}
```

Ha lefordítjuk eredményként megkapjuk a CSS file-t.


Ha jól megnézzük, képesek vagyunk egymásba ágyazni a szülő gyermek elemeket azzal, hogy a szülő kapcsos zárójelein belülre rakjuk a gyermek szelektorát.
Gyakorlatilag egy szóközzel összefűzi a sass compiler ezeket az egymásba ágyazott elemeket.
Mehetünk további mélységekig is:

```scss
body {
    nav {
         a {
            color: purple;
            display: inline-block;
         }
     }
}
```

Ez fordításkor " body nav a { } " szelektorrá alakul.
De mi a helyzet, ha például pszeudó szelektort akarok használni, ahol nem lehet szóköz a szelektor mellett?
Itt a kulcs karakter a **'&'**. Nézzünk egy gyors példát, ha egy elem ':hover' tulajdonságát szeretnénk változtatni:

```scss
body {
    nav {
         a {
            color: purple;
            display: inline-block;
               &:hover {
                color: red;
              }
           }
       }
}
```
Eddig csak az elemek közvetlen "html tag" szelektoraival játszottunk.
Nézzük meg, hogyan tudjuk láncszerűen összefűzni a class szelektorokat:
Egészítsük ki a html elemünket:

**index.html**:


```html
<nav class="nav">
      <a class="nav-link" href="">Link</a>
</nav>
```

**style.scss**:

```scss
.nav {
     color:purple;
      &-link {
        display: inline-block;
     }
}
```

*Megjegyzés: fontos itt a **'&'** karakter megfelelő használata, nehogy szóköz maradjon a ".nav" és a "-link" szó között!

Kimenete, ahogy lefordul **css** fájlra:

```css
.nav {
    color: purple;
}
.nav-link {
     display: inline-block;
}
```
Mint látjuk a Nesting segítségével megszünhet a szelektorok deklarásainak állandó ismétlődése.
Külön hangsúlyt kap a megfelelő elnevezés, mely könnyíti a szeleketorok egymásba fűzését.
Alábbi példában "class" szelektorokkal látjuk el a az egymásba ágyazott html-elemeket.
Minden elem klassz szelektorának elnevezése kiegészíti a szülő-elem szelektorát:

**index.html**:

```html
 <main class="main">
        <nav class="main-nav">
            <ul class="main-nav-wrapper">
                <li class="main-nav-wrapper-link">JS</li>
                <li class="main-nav-wrapper-link">HTML</li>
                <li class="main-nav-wrapper-link">CSS</li>
            </ul>
        </nav>
    </main>
```


Így még érthetőbben egymásba fűzhetőek az scss osztályban:


**style.scss**:

```scss
.main{
  &-nav {
    &-wrapper{
      &-link{
        color: red;
      }
    }
  }
}
```

Most ha ellátunk egy-két tulajdonsággal az egyes elemeket:

**style.scss**:

```scss
.main{
  width: 200px;
  margin: 2% auto;

  &-nav {
    text-align: center;

    &-wrapper{
      padding: 1rem;
      display: block;
      background: lightgray;

      &-link{
        color: red;
      }
    }
  }
}
```

Akkor lefordítás után rögtön látszani fog a különbség:

**style.css**:

```css
.main {
  width: 200px;
  margin: 2% auto;
}
.main-nav {
  text-align: center;
}
.main-nav-wrapper {
  padding: 1rem;
  display: block;
  background: lightgray;
}
.main-nav-wrapper-link {
  color: red;
}
```
Látjható, hogy az scss fájlban egy szelektoron belül helyezük el összefűzve a további szabályokat.
Ezt a CSS file-ben 4 hasonló elnevezésű de külön álló css szabállyal tudnánk megvalósítani.
Minél nagyobb a kód, annál inkább összetömöríti és olvashatóbban csoportosítja a css szabályokat a  Nesting eszköze.

Nestingról további opcionális leírás lásd.  [Nesting documentation](https://sass-lang.com/documentation/style-rules#nesting)

Bármikor, ha csak egy gyors tesztet szeretnék nézni valami scss kódrészlete, vagyis, hogy "fordulna le" css file-ra, akkor arra a legjobb eszköz a [SassMeister](https://www.sassmeister.com/)


![alt sass-mester](./img/sass-mester.png)

## A SCSS változók

Ha kicsit is van tapasztalatunk már valamelyik programozási nyelven, akkor már ismerjük a változók  hasznosságát.

Képzeljük el most egy pillanatra:

Egy vagy több hatalmas css file-ban dolgozunk. Legyen akár több ezer soros kód.
Legyenek benne olyan visszatérő értékek, elemek amelyeket több css szabály is alkalmaz.
Szeretnék ezeket az értékeket valamennyi elemre vonatkozóan egy időben változtatni, és figyelni, hogyan változik az oldal összképe.
Ha egyesével kellene kikeresni a közös tulajdonságokat, amin változtatni szeretnék, akkor ez nagyon időigényes lenne.

Ennek megkerülésére van lehetőség a változók használatával. Nézzük is meg, hogy deklarálok egy változót scss-bent:

``` $link-color: purple;```

A **'$'** jel itt a kulcs, majd a **'$'** jel + változó nevével tudom elkérni az értékét.
Egy gyors mini példa:

**style.scss**:

```scss
 $link-color: purple;
.nav {
     color: $link-color;
    &-link {
        display: inline-block;
      }
       h3 {
          color: $link-color;
      }
}
```

Ahogy lefordul CSS tartalomra:
**style.css**:

```css
.nav {
   color: purple;
}
.nav-link {
    display: inline-block;
}
.nav h3 {
    color: purple;
}
```

Láthatjuk, hogy bár az scss file-ban egy helyen deklaráljuk a változót, a css fájlban minden tulajdonság felveszi az értékét, amihez a változót rendeljük.

Kis kódban lehet, hogy elsőre nem tűnik ez az eszköz hasznosnak, de több ezer sor között, akár 10-15 értéket is meg tudunk változtatni egy helyen egy toll vonással.
Sőt ezeket a változókat ki lehet szervezni egy külön file-ba is.

Ezeknek a változóknak is vannak hatás köreik (**"scope"**)

1. globális változók (scss fájl elején, vagy máshonnan beimportálva bárhol elérhetőek)
2. helyi változók (egy szelektoron belül deklarálva csak a szelektoron belül elérhetőek)

#### ----------- Feladat: Nesting and variables ----------

- Épits fel egy oldal stílus jegyeit az scss segítségével.
- Feladat forrás: ```02-nesting-and-variables``` package
- Leírást és segédképet is itt találsz
- lépj be a terminalon a könyvtárba és onnan futtasd a scss compiler-t
- a ```dist/style/style.css``` file-ba kell majd fordítani a scss forrás file-t

**Megjegyzés**:

Ha színekkel dolgozunk mindig nagy kihívás lehet, hogy válasszunk ki összeillő színeket egy weboldalon (ha nincs megadva semmilyen séma)

Erre vannak online elérhető felületek, amik segítenek kiválasztani az ún. "szín-palettákat".
A feladatleírásban szereplő színkombinációkhoz a [Color Combo](https://www.colorcombos.com/) weboldalt használtuk segítségnek

#### --------------------------------------------------------------------

De mi  a helyzet ha nem csak egy értéket, változót szeretnék újrahasznosítani, hanem egy nagyobb kódrészletet, több css szabályt egyszerre?
Ebben lesz segítségünkre a **"mixin"** eszköze


## Mixin

Nézzük meg most, hogy milyen eszközzel tudunk több css szabályt, nagyobb kódrészletet újrahasznosítási céllal elmenteni.

Kulcsszavunk itt a **@mixin**

Legyen az a célunk, hogy létrehozunk egy olyan kódrészletet, ahol egy tartalmat középre zárunk, fehér színnel látjuk el benne a szöveg színét, és beállítjuk a betűméretét:

**style.scss**l.

```scss
@mixin add-basic-format(){
    text-align: center;
    color: white;
    font-size: 1.3rem;
}

.container {
    &-text-holder {
        p {
           @include add-basic-format();
           padding: 3px;
           width:70%;
           margin: 0 auto;
        }
    }
}
```
Mint látjuk egy ilyen kódrészlet deklarálása hasonló egy függvény létrehozásához. Alábbi dolgok szükségesek:

- @mixin kulcs szó
- a "mixinük" neve
- zárójel
- opcionálisan megadhatunk paramétert is a zárójelben
- { } jelek között megadjuk az értékeket

A meghívás, **mixin-re történő hivatkozás** kulcsszava az **@include** + mixin neve
Nézzünk egy példát **paraméter megadására** is:

```scss
@mixin add-basic-format($color){
    text-align: center;
    color: $color;
    font-size: 1.3rem;
}

.container {
    &-text-holder {
        p {
           @include add-basic-format(rgb(30,30,30));   // a zárójelben meg kell adnom az értéket a paraméternek
           padding: 3px;
           width:70%;
           margin: 0 auto;
        }
    }
}
```
Mint látjuk a '$' jellel tudok létrehozni bármennyi paramétert. Ha a példának megfelelően járok el, kötelező a paraméterek számainak megegyező számú értéket adnom a mixin meghívásakor.
Tehát, ha például két paraméterrel hozom létre a mixin-t, akkor amikor meghívom 2 értéket kell adnom a zárójelbe.
De van olyan lehetőségem, hogy ne kelljen minden esetben értéket adnom egy paraméternek?
Igen, nézzük meg az alábbi kódrészletet:

```scss
@mixin add-basic-format($color: rgb(30,30,30)){
    text-align: center;
    color: $color;
    font-size: 1.3rem;
}

.container {
    &-text-holder {
        p {
           @include add-basic-format();
           padding: 3px;
           width:70%;
           margin: 0 auto;
        }
        h3 {
          @include add-basic-format(black);

        }
    }
}
```
A kulcs momentum itt a mixin parameter létrehozásában van: ==>   **@mixin add-basic-format($color: rgb(30,30,30))**
Ha használjuk a kettős-pontot, és megadunk egy konkrét értéket, akkor  a mixin meghívásakor, ha nem adunk értéket, automatikusan ez az érték fog beállítódni a megfelelő tulajdonsághoz.

Nézzük meg milyen CSS kód generálódik le:

```css
.container-text-holder p {
   text-align: center;
   color: #1e1e1e;
   font-size: 1.3rem;
   padding: 3px;
   width: 70%;
   margin: 0 auto;
}

.container-text-holder h3 {
   text-align: center;
   color: black;
   font-size: 1.3rem;
}
```

Egyértelműen kiolvasható, hogy míg a "p" bekezdésünknél, ahol nem adtunk meg értéket paraméterként a mixin meghívásakor, ott a beállított alapérték állítódik be, míg a "h3" elemünk esetén meg a "black" szín, amit megadunk paraméterként.


Természetesen  bármennyi mixin-t használhatunk, importálhatunk más file-okból, amiben eltér a mixin egy függvénytől, hogy nincs visszatérési értéke, és nem szoktuk  bonyolultabb műveletek elvégzésére használni. (A függvényekre, importálásokra, hamarosan kitértünk)

További opciónális olvasni való a [Mixinek használatáról](https://sass-lang.com/documentation/at-rules/mixin)

## Más file-ból történő importálás
Ha teljesen ki szeretnék használni egy kódrészlet újrafelhasználását, ahhoz arra van szükségünk, hogy ne csak egy file-on belül, hanem akár több file-ban is fel tudjunk használni egy részletet.

Itt jönnek képbe az SCSS file-ok importálása.


### Az @import


Importálás volt az első eszköz, hogy felhasználjuk egy másik scss file-ben létrejött kódot. Egyik leggyakoribb felhasználásai, ha egy scss file-ban visszatérő változókat, mixin()-neket, vagy függvényeket szeretnék tárolni.

Nincs más dolgunk, mint így az összetartozó elemeket csoportosítva külön file-okban tároljuk. Majd a szükséges "fő" scss file-ba beimportáljuk.

Tételezzük fel, hogy van egy **_mixin.scss** és egy **_color.scss** file tárolni ezeket az értékeket.

Az alábbi módon tudjuk más file-ban felhasználni az elemeiket:

 **style.scss**:

 ```scss
@import 'color';
@import 'mixin';

header {
     @include setBanner(150px, $banner-background-color);
}
```

Az **'@import'** paranccsal tudjuk elérni a másik fájlban deklarált elemeket. Nem kell a file (scss) kiterjesztését kiírni.
Példánkban a ```setBanner()``` mixin-t és a ```$banner-background-color``` változót saját nevével fel is tudjuk használni.

De van itt egy bökkenő. Mi a helyzet akkor, ha több forrásban is ugyan olyan néven vannak változóink, függvényeink?
Ha egy elnevezésre hivatkozunk, tudja a file, hogy melyik forrásra vonatkozzon?


### A @use

Ez a fajta importálási mód nyújt lehetőséget arra, hogy egy külön "name space", vagyis elnevezés jöjjön létre minden scss file-ra.
Nézzük meg, hogyan tudjuk felhasználni ezt az importálás során:


```scss
@use 'basic-setting' as b;
header {
     @include b.setBanner(150px, b.$banner-background-color);
}
```

A **@use** paranccsal szintén importálódik egy fájl tartalom. Esetünkben ha "basic-setting" a file neve akkor: ```basic-setting.setBanner()``` paranccsal érjük el a "setBanner" nevű mixin-t.

Használhatjuk az 'as' parancsot (Lásd fenti példa), akkor az utána megadott névvel tudunk hivatkozni.

Ekkor tetszőlegesen már szét tudjuk válogatni a parancsokat, több file-t is tudunk importálni a use paranccsal


#### ----------- Feladat: Mixin and Imports ----------

- Épits fel egy oldal stílus jegyeit az scss segítségével.
- Feladat forrás: ```03-mixin-and-imports``` package
- Leírást és segédképet is itt találsz
- lépj be a terminalon a könyvtárba és onnan futtasd a scss compiler-t
- a ```dist/style/style.css``` file-ba kell majd fordítani a scss forrás file-t

#### -------------------------------------------------------------

További opcionális olvasni való az importálási lehetőségekről:
 - [sass @use](https://sass-lang.com/documentation/at-rules/use)
 - [sass @import](https://sass-lang.com/documentation/at-rules/import)

#### ----------------- Opciónális (lecke) feladat --------------------

- Készítsd el egy randi app bemutatkozó oldalát
- FELADAT leírása, forrásai, mintaképei a ```04-homework-and-imports``` mappában vannak
- a szokásos módon indítsd el a compilert

Megjegyz: Nem csak a színek a képek, hanem a betűtípusok használata is nagyon fel tudja dobni egy oldal megjelenését

Ajánlott legalább két betűtípus használata. Alapséma szokott lenni, hogy a címekben, "Header"-ekben van egy egységes betűtípus,
és az egyéb szöveges tartalomban meg egy eltérő típus.

Amire figyelni kell:
- "websafe" legyen egy betűtípus, vagyis a böngésző támogassa
- kezelje az ékezetek, ha szükséges a nyelvhez

A leggyakoribb biztonságos betűtípusról egy lista => [WebSafe font lista](https://www.w3schools.com/cssref/css_websafe_fonts.asp)

Van még egy lehetőségünk...

####  Google Fonts api

Beimportálhatunk egyedi betűtípust a google-nek hála.
Annyi a feladatunk, hogy kiválasszul a tetszőleges családod, ellenőrizzük, hogy támogatja-e az ékezeteket, ha szükséges.


Két módon lehet behúzni a betűket:

- @import segítségével (telepítjük a betűkészletet az alkalmazásunkba)
- link-et használunk (index.html "head" elemében)

A google font api elérhető itt => [Google Fonts](https://fonts.google.com/)

Az összeillő betűpárosítás kiválaszátást segítheti ez az oldal => [Font Pairings](https://www.bypeople.com/online-font-pairing-html-css/)

#### ---------------------------------------------------------------------------

### License



Copyright © Progmasters (QTC Kft.), 2016-2019. All rights reserved. No part or the whole of this Teaching Material (TM) may be reproduced, copied, distributed, publicly performed, disseminated to the public, adapted or transmitted in any form or by any means, including photocopying, recording, or other electronic or mechanical methods, without the prior written permission of QTC Kft. This TM may only be used for the purposes of teaching exclusively by QTC Kft. and studying exclusively by QTC Kft.’s students and for no other purposes by any parties other than QTC Kft. This TM shall be kept confidential and shall not be made public or made available or disclosed to any unauthorized person. Any dispute or claim arising out of the breach of these provisions shall be governed by and construed in accordance with the laws of Hungary.
