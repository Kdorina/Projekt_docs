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
|-config
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
* AdminController -
* AuthController -
* BaseController -
* FileController -
* NoteController -
* SubjectController -

## Adatbázis terv
