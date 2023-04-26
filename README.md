# Felhasználói dokumentáció
## Backend - Diák Felhő

## Projekt leírás
* Tanulók számára készített almalmazás, mely bíztosítja számukra az aznap kapott jegyük/teendőik/jegyzeteikről készített képek tárolását, azoknak a módosítását, esetleges törlését és a statisztikai adatok visszajelzését. Ez álltal a tanuló vissza tudja követni kapott jegyeit és tiszában van tanulmányi eredményéről.

## Használt technológiák
* Laravel
* MYSQL szerver

## Projekt elérés
| Leírás | HELY | URL |
| ------ | ------ | ------ |
|Backend projekt | GitHub | https://github.com/Kdorina/Projekt_laravel|
|Insomnia fájl a projekthez |Github| https://github.com/Kdorina/Projekt_laravel|

## Backend indítás előtti teendők

A projekt klónozásához szükséges lesz a git parancs használatára. Melyet az alábbi linken tudtok letölteni.

| Leírás | URL|
| ------ | ------ | 
|Git parancs letöltési link|https://git-scm.com|

```
cmd —> terminál megnyítása
git clone -> hogy a projektet sikeresen le tudjuk klónozni ki kell másolni a projekt web url-jét majd a git clone után beillesztve kiadni a parancsot
cd Projekt_laravel -> szükséges parancs a leklónozott könyvtárba lépéshez
.env -> file létrehozása
```

Ahoz hogy kitudjuk sikeresen adni a composer parancsot, le kell töltenünk az alábbi linkről
| Leírás | URL|
| ------ | ------ | 
|Composer letöltés link|https://getcomposer.org/download/|

```
composer install -> vendor létrahozása
php artisan key:generate -> kulcs legenerálása
php artisan migrate -> adatbázis táblák létrehozása
php artisan storage:link -> storage létrehozása
```

Git parancson kívűl szükség lesz egy XAMMP-ra, hogy hozzáférjunk az adatbázishoz és egy Insomniára, hogy weboldal nélkül is hozzáférjunk a funkiókhoz, 
| Leírás | URL|
| ------ | ------ | 
|Insomnia letöltés link| https://insomnia.rest/download |
|XAMPP letöltési link| https://www.apachefriends.org/hu/index.html |
## A backend indítása 
```
php artisan serve
```
Ezen a porton fog futni a program.
```
127.0.0.1:8000
```
