# Felhasználói dokumentáció


## Függőségek telepítése

    cmd
    cd Projekt_laravel
    composer install

## A backend indítása / ahoz szükséges parancsok

    php artisan migrate
    php artisan key:generate
    php artisan storage:link
    php artisan serve

## Backend projekt könyvtár szerkezet

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

## AuthController

## AdminController

## BaseController

## FileController

## NoteController

## SubjectController

## Adatbázis terv

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
