# Felhasználói dokumentáció


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
    * AdminController - Az admin authentikációt kezeli.
    * AuthController - A user authentikációt kezeli, admin számára user adatokat szűr (tanulók adatai,iskolák, átlag életkor, nemek aránya, ).
    * BaseController - Üzenet küldésre szolgál.
    * FileController - Képek/leírások megjelenítésére, felvételére(tárolására),felhasználó általl felvett mennyiség megjelenítése és törlésére szolgál.
    * NoteController - Jegyzet megjelenítésére, felvételére, felhasználó általl felvett mennyiség megjelenítése és törlésre szolgál.
    * SubjectController - tantárgy/jegyek megjelenítése , felvétele, módosítása, törlése, a felhasználó adatainak szűrése(bejelentkezett felhasználó felvett tantárgyai/jegyei, össz átlagának/ tantárgyankénti átlag meghatározás, eltárolt tantárgyainak megjelenítése) a feladata.

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

    Az AuthController osztély végzi a regisztrációt, bejelentkezést, kijelentekzést és az admin számára adatok lekérdezését.

 Metódusok: 

    login()
    Célja a felhasználó bejelentkeztetése. A bejelentkezési folyamat során megadott email cím és jelszó ellenőrzése történik meg, majd a megfelelő adatok esetén az alkalmazás belépteti a felhasználót.A metódus egy Request objektumot kap, mely tartalmazza a bejelentkezéshez szükséges adatokat, email cím, jelszó. Ha a felhasználó megadott adatai nem megfelelőek, akkor a login metódus hibára fut, és hiba üzenetet ad.

    register()
    Feladata az új felhasználók felvétele/ a felhasználó által megadott adatok ellenőrzése, majd a helyesen megadott adatok elmetése az adatbázisban. A metódus egy Request objektumot kap, mely tartalmazza a bejelentkezéshez szükséges adatokat, név, intézmény név, email cím, születési dátum, nem és jelszó. Az adatok ellenőrzése során validációs szabályokat használ, mely biztosítja, hogy  a megadott adatok helyesek legyenek, ha helyesek akkor a felhasználót felveszi az adatbázisba. Sikertelen regisztráció esetén hibára fut, hiba üzenetet ad vissza.

    logout()
    Feladata a felhasználó kijelentkeztetése az alkalmazásból. CurrentAccessToken() metódus meghívásával , megtalálja az aktuális felhasználóhoz hozzátartózó tokent, majd azt az adatbáziból kitörli. Sikeres kijelentekztetés esetén , üzenettel tér vissza.

    getUsers()

    countUsers()

    userAvgAge()

    getWomens()

    getMens()

    getElse()

    allBuilding()
    
    

## AdminController
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
Metódusok:

    index()
    Feladata vissza adni a felhasználónak az eddig felvett összes adatát. Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál. Ha a visszatérési érték igaz lesz , akkor az aktuális felhasználó megkapja az eddig felvett képeit. Ha hamis , a felhasználó nincs bejelentkezve, tehát nem érkeznek meg az adatai.

    store()
    Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál.
    $id = Auth::user()->getId() -> új kép sikeres felvétele esetén, a bejelentkezett felhasználó id-ját eltárolja $id-ban majd ezt kapja meg a user_id.
    A metódus Request objektumot kap, melynek description, imgpath mezője elvárt.
    if(!$request->hasFile('imgpath') && !$request->file('imgpath')->isValid()) függvénny arra szolgál, ha az elvárt kép nem lett kiválasztva, akkor hibára fut akkor a hiba üzenet "error":" kérlek válaszd ki a képet". Ha sikeres a validáció, akkor újleírást és képet getClientOriginalName() függvény használatával a feltöltött képet eredeti nevén menti el az adatbázisban, majd a storeAs() függvénnyel eltárolja a storgage mappa    'public' al mappájában. Az adatokat visszaküldi a FileResource használatával, majd 'sikeres felvétel' üzenetet ad vissza.

    destroy()
    Feladata törölni a kiválasztott id-hoz tartalmazó képet/leírást. 
    Storage::exists('public/'. $file->imgpath) függvénny célja meghatározni hogy a kiválasztott kép szereple-e a public mappában , ha ez igaz akkor 
    Storage::delete('public/'. $file->imgpath) tehát törli a mappából a képet és ezzel párhuzamosan az adatbázisból is törlődnek az adatok. 
    Sikeres törlés esetén "Sikeresen törölve" üzenetet ad vissza. Ha nem létezik a kép a storage mappában akkor is törli a az adatbáziban lévő kiválaszott adatot, hiba üzenetként "Nem létezik ilyen kép a storage mappában" ad vissza.

    countFile()
     Feladata vissza adni a felhasználónak az eddig felvett összes adatát. Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál. Ha a visszatérési érték igaz lesz , akkor az aktuális felhasználó adataival tér vissza. Query Builder használatával lekérdezés történik. A where() metódusnak megadott feltétel ('user_id'=>$user_id ), arra szolgál, hogy csak azokat a rekordokat keresi, amelyeknek a 'user_id' mezője megegyezik a '$user_id' változó értékével. majd select() metódussal lekérdezi a files táblából az id-kat, majd ezt követően count() metódus segítségével meszámolja a lekérdezett adatokat. 


## NoteController
Metódusok:

    index()
    Feladata vissza adni a felhasználónak az eddig felvett összes adatát. Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál. Ha a visszatérési érték igaz lesz , akkor az aktuális felhasználó megkapja az eddig felvett teendőit. Ha hamis , a felhasználó nincs bejelentkezve, tehát nem érkeznek meg az adatai.

    store()
    Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál.
    $id = Auth::user()->getId() -> új kép sikeres felvétele esetén, a bejelentkezett felhasználó id-ját eltárolja $id-ban majd ezt kapja meg a user_id.
    A metódus Request objektumot kap, melynek note mezője elvárt. Ha a validáció sikertelen, akkor egy hiba üzenet kerül vissza küldésre sendError() metódussal. Sikeres validáció ésetén létrehoza a felvett 'teendőt' majd az adatokat visszaküldi a NoteResource használatával, majd 'sikeres feltöltés' üzenetet ad vissza.

    destroy()
    Feladata törölni a kiválasztott id-hoz tartalmazó 'teendőt'. 
    Sikeres törlés esetén "Note sikeresen törölve" üzenetet ad vissza. Ha nem létezik ilyen id-jú note akkor hibára fut.

    countNotes()
     Feladata vissza adni a felhasználónak az eddig felvett összes adatát. Auth::check() -> a bejelentkezett felhasználó ellenőrzésére szolgál. Ha a visszatérési érték igaz lesz , akkor az aktuális felhasználó adataival tér vissza. Query Builder használatával lekérdezés történik. A where() metódusnak megadott feltétel ('user_id'=>$user_id ), arra szolgál, hogy csak azokat a rekordokat keresi, amelyeknek a 'user_id' mezője megegyezik a '$user_id' változó értékével. majd select() metódussal lekérdezi a files táblából az id-kat, majd ezt követően count() metódus segítségével meszámolja a lekérdezett adatokat. 

## SubjectController
Metódusok:

    index()

    update()

    store()

    destroy()

    avarageAllSubject()

    mySubject()

    avgGradeFromAddSubjects()

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
