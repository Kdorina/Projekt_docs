# Felhasználói dokumentáció


## Projekt elérés
| HELY | URL |
| ------ | ------ |
| GitHub | https://github.com/Kdorina/Projekt_laravel|
## Backend indítás előtti teendők

    cmd —> terminál megnyítása
    cd Projekt_laravel -> szükséges parancs a könyvtárba lépéshez
    composer install -> vendor létrahozása
    .env -> file létrehozása
    php artisan key:generate -> kulcs legenerálása
    php artisan storage:link -> storage létrehozása
    php artisan migrate -> adatbázis táblák létrehozása

## A backend indítása / ahoz szükséges parancsok

    php artisan serve

Ezen a porton fog futni a program.
```sh
127.0.0.1:8000
```

## Backend projekt könyvtárszerkezet
A backend egy olyan php alapú laravel keretrendszerben elkészített alkalamzás, melyet REST API szolgál ki.

    Projekt_laravel/
        |-app/
            |-Http/
                |-Controllers/
                    |-AdminController
                    |-AuthController
                    |-BaseController
                    |-Controller
                    |-FileController
                    |-NoteController
                    |-SubjectController

                |-Middleware/

                |-Resources/
                    |-Admin
                    |-File
                    |-Note
                    |-Subject
                    |-User

            |-Models/
                |-Admin
                |-File
                |-Note
                |-Subject
                |-User

        |-bootstrap/ 
        |-config/
        |-database/
            |-factories/

            |-migrations/
                |-users
                |-subjects
                |-files
                |-admins
                |-notes
        |-public/
        |-resources/

        |-routes/
            |-api.php

        |-storage/
        |-tests/
        |-vendor/
## Létrehozott kontrollerek:

> AdminController - Az admin authentikációt kezeli.
>
> AuthController - A user authentikációt kezeli, admin számára user adatokat szűr (tanulók adatai,iskolák, átlag életkor, nemek aránya, ).
>
> BaseController - Üzenet küldésre szolgál.
>
> FileController - Képek/leírások megjelenítésére, felvételére(tárolására),felhasználó általl felvett mennyiség megjelenítése és törlésére szolgál.
>
> NoteController - Jegyzet megjelenítésére, felvételére, felhasználó általl felvett mennyiség megjelenítése és törlésre szolgál.
>
> SubjectController - tantárgy/jegyek megjelenítése , felvétele, módosítása, törlése, a felhasználó adatainak szűrése(bejelentkezett felhasználó felvett tantárgyai/jegyei, össz átlagának/ tantárgyankénti átlag meghatározás, eltárolt tantárgyainak megjelenítése) a feladata.

Az adatbázis feltöltése insomnian keresztűl történik, mely a documnetáció mellett a projekthez elkészített insomnia fájl másolata is megtalálható.

## Táblák (modellek) közti kapcsolat: 
    * User 
        - hasMany -> subject
        - hasMany -> files
        - hasMany -> notes

    * Subject
        - belongsTo -> User

    * File
        - belongsTo -> User

    * Note
        - belongsTo -> User

## Resources

    Admin
    File
    Note
    Subject
    User

Az adatok átadása frontend részére

# Controllerek
Az authentikáció érdekében Sanctum csomag lett feltelepítve
## AuthController

    Az AuthController osztály végzi a regisztrációt, bejelentkezést, kijelentekzést és az admin számára adatok lekérdezését.

 Metódusok:

    login()
    Célja a felhasználó bejelentkeztetése. A bejelentkezési folyamat során megadott email cím és jelszó ellenőrzése történik meg, majd a megfelelő adatok esetén az alkalmazás belépteti a felhasználót.A metódus egy Request objektumot kap, mely tartalmazza a bejelentkezéshez szükséges adatokat, email cím, jelszó. Ha a felhasználó megadott adatai nem megfelelőek, akkor a login metódus hibára fut, és hiba üzenetet ad.

    register()
    Feladata az új felhasználók felvétele/ a felhasználó által megadott adatok ellenőrzése, majd a helyesen megadott adatok elmetése az adatbázisban. A metódus egy Request objektumot kap, mely tartalmazza a bejelentkezéshez szükséges adatokat, név, intézmény név, email cím, születési dátum, nem és jelszó. Az adatok ellenőrzése során validációs szabályokat használ, mely biztosítja, hogy  a megadott adatok helyesek legyenek, ha helyesek akkor a felhasználót felveszi az adatbázisba. Sikertelen regisztráció esetén hibára fut, hiba üzenetet ad vissza.

    logout()
    Feladata a felhasználó kijelentkeztetése az alkalmazásból. CurrentAccessToken() metódus meghívásával , megtalálja az aktuális felhasználóhoz hozzátartózó tokent, majd azt az adatbáziból kitörli. Sikeres kijelentekztetés esetén , üzenettel tér vissza.

    getUsers()
    Feladata az adatbázis táblában szereplő felhasználók listájának vissza adása.

    countUsers()
    Feladata a users adatbázis táblában eltárolt felhasználók megszámlálása. Query Builder használatával lekérdezés történik. table() metódus a megadott 'users' adatbázis táblában keres. Select() metódussal id-ra szűr, majd count('id') metódus megszámlálja az összes felhasználó id-ját. A $sum-ban eltárolt adattal tér vissza.

    userAvgAge()
    Feladata az admin oldalhoz való felhasználás érdekében a felhasználók átlag korának lekérdezése. Builder használatával lekérdezés történik. Select() metódusban sql lekérdezés van alkalmazva adatbázis táblában keres. SELECT ROUND(AVG(YEAR(CURDATE())-Year(date_of_birth))) as "kor" FROM users -> CURDATE() az aktuális dátumot adja vissza melyet a YEAR() metódus csak az év értéket adja vissza, majd a regisztráció során megadott 'date_of_birth' dátum Year() metódusba helyezése, mely szintén csak az évet veszi figyelembe. Az így kapott két évszám kivonásra kerül egymásból. Avg() metódussal a kapott számot átlagolja, round() metódus pedig kerekíti az értéket, majd az így kapott számmal tér vissza mint "kor".

    getWomens()
    Női felhasználók lekérdezése Admin oldalhoz való felhasználás érdekében.
    Builder használatával lekérdezés történik. table() metódus a megadott 'users' adatbázis táblában keres. Where() metódus a megadott feltételek alapján keres ("gender, "nő") .Select() metódussal id-ra szűr. Így megkapjuk az összes női felhasználót, majd a kapott adatot count() metódus segítségével az összes női felhasználó id-ját megszámlálja, és visszatér vele.

    getMens()
    Férfi felhasználók lekérdezése Admin oldalhoz való felhasználás érdekében.
    Builder használatával lekérdezés történik. table() metódus a megadott 'users' adatbázis táblában keres. Where() metódus a megadott feltételek alapján keres ("gender, "férfi") .Select() metódussal id-ra szűr. Így megkapjuk az összes férfi felhasználót, majd a kapott adatot count() metódus segítségével az összes férfi felhasználó id-ját megszámlálja, és visszatér vele.

    getElse()
    Egyéb felhasználók lekérdezése Admin oldalhoz való felhasználás érdekében.
    Builder használatával lekérdezés történik. table() metódus a megadott 'users' adatbázis táblában keres. Where() metódus a megadott feltételek alapján keres ("gender, "egyéb") .Select() metódussal id-ra szűr. Így megkapjuk az összes egyéb felhasználót, majd a kapott adatot count() metódus segítségével az összes egyéb felhasználó id-ját megszámlálja, get() metódus vissza adja az értékeket.

    allBuilding()
    Feladata a felhasználók által megadott intézmények lekérdezése.
    Builder használatával lekérdezés történik. table() metódus a megadott 'users' adatbázis táblában keres. Select() metódus "buildingName" -re, mely a regisztráció során felvett intézmény neveire szűr. GroupBy() metódus a kapott adatokat csoportosítja "buildingName" szerint, majd vissza tér az eredménnyel.
    
    

## AdminController
  Az AdminController osztály végzi a regisztrációt, bejelentkezést, kijelentekzést.

 Metódusok: 

    login()
    Célja a felhasználó bejelentkeztetése. A bejelentkezési folyamat során megadott email cím és jelszó ellenőrzése történik meg, majd a megfelelő adatok esetén az alkalmazás belépteti a felhasználót.A metódus egy Request objektumot kap, mely tartalmazza a bejelentkezéshez szükséges adatokat, email cím, jelszó. Ha a felhasználó megadott adatai nem megfelelőek, akkor a login metódus hibára fut, és hiba üzenetet ad.

    register()
    Feladata az új felhasználók felvétele/ a felhasználó által megadott adatok ellenőrzése, majd a helyesen megadott adatok elmetése az adatbázisban. A metódus egy Request objektumot kap, mely tartalmazza a bejelentkezéshez szükséges adatokat, név, email cím és jelszó.Az adatok ellenőrzése során validációs szabályokat használ, mely biztosítja, hogy  a megadott adatok helyesek legyenek, ha helyesek akkor a felhasználót felveszi az adatbázisba. Sikertelen regisztráció esetén hibára fut, hiba üzenetet ad vissza.

    logout()
    Feladata a felhasználó kijelentkeztetése az alkalmazásból. CurrentAccessToken() metódus meghívásával , megtalálja az aktuális felhasználóhoz hozzátartózó tokent, majd azt az adatbáziból kitörli. Sikeres kijelentekztetés esetén , üzenettel tér vissza.

## BaseController

    sendResponse()
    Ez a metódus egy JSON választ küld vissza, mely success, data , message mezőt tartalmaz.

    sendError()
    Ez a metódus egy JSON választ küld vissza, mely hiba üzenetet generál , error, errorMessage és az aktuális code mezőt tartalmazza. A metódus alapértelmezettként a 404 hibakódot adja vissza.

## FileController
  A FileController osztály végzi a képek + leírások megjelenítését, felvételét, törlését a felhasználó számára, illetve a felvett fájlok megszámlálásának lekérdezését.

Metódusok:

    index()
    Feladata vissza adni a felhasználónak az eddig felvett összes adatát. Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál. Ha a visszatérési érték igaz lesz , akkor az aktuális felhasználó megkapja az eddig felvett képeit. Ha hamis , a felhasználó nincs bejelentkezve, tehát nem érkeznek meg az adatai.

    store()
    Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál.
    $id = Auth::user()->getId() -> új kép sikeres felvétele esetén, a bejelentkezett felhasználó id-ját eltárolja $id-ban majd ezt kapja meg a user_id.
    A metódus Request objektumot kap, melynek description, imgpath mezője elvárt.
    if(!$request->hasFile('imgpath') && !$request->file('imgpath')->isValid()) függvénny arra szolgál, ha az elvárt kép nem lett kiválasztva, akkor hibára fut akkor a hiba üzenet "error":" kérlek válaszd ki a képet". Ha sikeres a validáció, akkor újleírást és képet getClientOriginalName() függvény használatával a feltöltött képet eredeti nevén menti el az adatbázisban, majd a storeAs() függvénnyel eltárolja a storgage mappa 'public' almappájában. Az adatokat visszaküldi a FileResource használatával, majd 'Sikeres felvétel' üzenetet ad vissza.

    destroy()
    Feladata törölni a kiválasztott id-hoz tartalmazó képet/leírást. 
    Storage::exists('public/'. $file->imgpath) függvénny célja meghatározni hogy a kiválasztott kép szereple-e a public mappában , ha ez igaz akkor 
    Storage::delete('public/'. $file->imgpath) tehát törli a mappából a képet és ezzel párhuzamosan az adatbázisból is törlődnek az adatok. 
    Sikeres törlés esetén "Sikeresen törölve" üzenetet ad vissza. Ha nem létezik a kép a storage mappában akkor is törli a az adatbáziban lévő kiválaszott adatot, hiba üzenetként "Nem létezik ilyen kép a storage mappában" ad vissza.

    countFile()
    Feladata vissza adni a felhasználónak az eddig felvett összes adatát. Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál. Ha a visszatérési érték igaz lesz , akkor az aktuális felhasználó adataival tér vissza. Query Builder használatával lekérdezés történik. A where() metódusnak megadott feltétel ('user_id'=>$user_id ), arra szolgál, hogy csak azokat a rekordokat keresi, amelyeknek a 'user_id' mezője megegyezik a '$user_id' változó értékével. majd select() metódussal lekérdezi a files táblából az id-kat, majd ezt követően count() metódus segítségével meszámolja a lekérdezett adatokat. 

## NoteController

    A NoteController osztály végzi a teendők megjelenítését, felvételét, törlését a felhasználó számára, illetve a felvett teendők megszámlálásának lekérdezését.

Metódusok:

    index()
    Feladata vissza adni a felhasználónak az eddig felvett összes adatát. Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál. Ha a visszatérési érték igaz lesz , akkor az aktuális felhasználó megkapja az eddig felvett teendőit. Ha hamis , a felhasználó nincs bejelentkezve, tehát nem érkeznek meg az adatai.

    store()
    Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál.
    $id = Auth::user()->getId() -> új note sikeres felvétele esetén, a bejelentkezett felhasználó id-ját eltárolja $id-ban majd ezt kapja meg a user_id.
    A metódus Request objektumot kap, melynek note mezője elvárt. Ha a validáció sikertelen, akkor egy hiba üzenet kerül vissza küldésre sendError() metódussal. Sikeres validáció ésetén létrehoza a felvett 'teendőt' majd az adatokat visszaküldi a NoteResource használatával, majd 'Sikeres feltöltés' üzenetet ad vissza.

    destroy()
    Feladata törölni a kiválasztott id-hoz tartalmazó 'teendőt'. 
    Sikeres törlés esetén "Note sikeresen törölve" üzenetet ad vissza. Ha nem létezik ilyen id-jú note akkor hibára fut.

    countNotes()
    Feladata vissza adni a felhasználónak az eddig felvett összes adatát. Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál. Ha a visszatérési érték igaz lesz , akkor az aktuális felhasználó adataival tér vissza. Query Builder használatával lekérdezés történik. A where() metódusnak megadott feltétel ('user_id'=>$user_id ), arra szolgál, hogy csak azokat a rekordokat keresi, amelyeknek a 'user_id' mezője megegyezik a '$user_id' változó értékével. select() metódussal lekérdezi a files táblából az id-kat, majd ezt követően count() metódus segítségével meszámolja a lekérdezett adatokat. 

## SubjectController
    
    A SubjectController osztály végzi a tantárgyak + jegyek megjelenítését, felvételét, frissítését, törlését , illetve a felhasználó számára való átlag, tantárgy lekérdezést.

Metódusok:

    index()
    Feladata vissza adni a felhasználónak az eddig felvett összes adatát. Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál. Ha a visszatérési érték igaz lesz , akkor az aktuális felhasználó megkapja az eddig felvett teendőit. Ha hamis , a felhasználó nincs bejelentkezve, tehát nem érkeznek meg az adatai.
    Query Builder használatával lekérdezés történik. A where() metódusnak megadott feltétel ('user_id'=>$user_id ), arra szolgál, hogy csak azokat a rekordokat keresi, amelyeknek a 'user_id' mezője megegyezik a '$user_id' változó értékével, majd get() metódus lekéri az adatbázis táblából a user_id-hoz tartozó összes adatot.

    store()
    Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál.
    $id = Auth::user()->getId() -> új subjet,grade sikeres felvétele esetén, a bejelentkezett felhasználó id-ját eltárolja $id-ban majd ezt kapja meg a user_id.
    A metódus Request objektumot kap, melynek subject,grade mezője elvárt. Ha a validáció sikertelen, akkor egy hiba üzenet kerül vissza küldésre sendError() metódussal 'Hiba!, Sikertelen felvétel'. Sikeres validáció ésetén létrehoza a felvett (subject, grade) majd az adatokat visszaküldi a SubjectResource használatával, majd 'Sikeres felvétel' üzenetet ad vissza.

    update()
    Feladata a kiválasztott id-hoz tartozó adatok frissítése (módosítása). A metódus Request objektumot kap, melynek subject, grade mezője elvárt. Ha a validáció sikertelen, akkor egy hiba üzenet kerül vissza küldésre sendError() metódussal 'Hiba! Szerkeztés sikertelen'. Ha a megadott adatok helyesek , akkor $request által kapott adatok alapján az adatbázis táblában frissíti a kiválasztott id-jú adatokat, majd vissza tér a frissített adattal.

    destroy()
    Feladata törölni a kiválasztott id-hoz tartalmazó 'subjet, grade' adatot. 
    Sikeres törlés esetén "Tantárgy törölve" üzenetet ad vissza. Ha nem létezik ilyen id-jú note akkor hibára fut.

    avarageAllSubject()
    Feladata vissza adni a felhasználónak az eddig feddigi felvett tantárgyainak átlagát. Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál. Ha a visszatérési érték igaz lesz , akkor az aktuális felhasználó adataival tér vissza. Query Builder használatával 2 darab lekérdezés történik. A where() metódusnak megadott feltétel ('user_id'=>$user_id ), arra szolgál, hogy csak azokat a rekordokat keresi, amelyeknek a 'user_id' mezője megegyezik a '$user_id' változó értékével.Az egyik lekérdezés sum() metódus segítségével összeadja a grade mező számait.
    A másik lekérdezés select() metódussal lekérdezi a subjects táblából a grade-eket, majd ezt követően count() metódus segítségével meszámolja a lekérdezett adatokat. 
    Az eltárolt eredmények, egy $arg változóban vannak eltárolva, melyben átlag számítási képlet van alkalmazva. A round() metódus segítségével 2 tizedesjegyre kerekíti a kapott eredmény, majd ezzel tér vissza.

    mySubject()
    Feladata vissza adni a felhasználónak az eddigi felvett tantárgyainak visszaadása. Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál. Ha a visszatérési érték igaz lesz , akkor az aktuális felhasználó adataival tér vissza. Query Builder használatával lekérdezés történik. A where() metódusnak megadott feltétel ('user_id'=>$user_id ), arra szolgál, hogy csak azokat a rekordokat keresi, amelyeknek a 'user_id' mezője megegyezik a '$user_id' változó értékével. A select() metódussal lekérdezi a subjects táblából a subject-eket, majd ezt követően groupBy() metódus segítségével csoportosítja 'subject-ek' alapján a lekérdezett adatokat. A metódus a kapott eredménnyel tér vissza.

    avgGradeFromAddSubjects()
    Feladata vissza adni a felhasználónak az eddig felvett jegyeinek átlagát tantárgyaként. Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál. Ha a visszatérési érték igaz lesz , akkor az aktuális felhasználó adataival tér vissza. Query Builder használatával lekérdezés történik. A where() metódusnak megadott feltétel ('user_id'=>$user_id ), arra szolgál, hogy csak azokat a rekordokat keresi, amelyeknek a 'user_id' mezője megegyezik a '$user_id' változó értékével. A select() metódussal lekérdezi a subjects táblából a subject-eket és a user_id-kat. DB::raw metódussal átlagszámítás alkalmazása, jegy darabszám meghatározása ('(sum(grade)/count(subject))as atlag, count(subject) as jegydb'), majd ezt követően groupBy() metódus segítségével csoportosítja 'subject-ek' és a 'user_id-k' alapján a lekérdezett adatokat. A metódus a kapott eredménnyel tér vissza a végén.


## Útvonalak

 * Admin

    | Metódus | Elérés  | Kontroller | Védett |
    | ------- | ------- | ---------- |----------- |
    | POST    | adminReg| AdminController| Nem |
    | POST    | adminLog| AdminController| Nem |
    | POST    | adminLogout| AdminController| Igen |
    | GET    | user| AuthController| Nem |
    | GET    | users/{id}| AuthController| Nem |
    | GET    | sum| AuthController| Nem |
    | GET    | womens| AuthController| Nem |
    | GET    | mens| AuthController| Nem |
    | GET    | else| AuthController| Nem |
    | GET    | allBuilding| AuthController| Nem |
    | GET    | usersSubjects| SubjectController| Nem |

* User

    | Metódus | Elérés  | Kontroller | Védett |
    | ------- | ------- | ---------- |----------- |
    | POST    | register| AuthController| Nem |
    | POST    | login| AuthController| Nem |
    | POST    | logout| AuthController| Igen |
    
* Subject

    | Metódus | Elérés  | Kontroller | Védett |
    | ------- | ------- | ---------- |----------- |
    | GET    | subject| SubjectController| igen |
    | DELETE    | deleteSubject/{id}| SubjectController| igen |
    | PUT    | updateSubject/{id}| SubjectController| Igen |
    | GET    | argAll| SubjectController| Igen |
    | GET    | mySubject| SubjectController| Igen |
    | GET    | grade| SubjectController| Igen |
    | GET    | showSubject/{id}| SubjectController| Nem |

* File

    | Metódus | Elérés  | Kontroller | Védett |
    | ------- | ------- | ---------- |----------- |
    | GET    | image| FileController| igen |
    | POST    | images| FileController| Igen |
    | DELETE    | deleteImages/{id}| FileController| igen |
    | Get    | countfile| FileController| Igen |
* Note

    | Metódus | Elérés  | Kontroller | Védett |
    | ------- | ------- | ---------- |----------- |
    | GET    | note| NoteController| igen |
    | POST    | notes| NoteController| Igen |
    | DELETE    | deleteNotes/{id}| NoteController| igen |
    | Get    | countNote| NoteController| Igen |
