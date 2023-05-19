# laravel-getting-started-package

## Init the Package

    $ composer init

then type the *vendor/package-name*. some description about your package, the *type* (here just put **library**), the licence, and confirm you want to use *src* directory as your *psr-4* source directory.

For now, just type **no** for the two questions about dependencies.

## Creating the GettingStarted Class

Inside *scr/* create a file named *GettingStarted.php*, and paste the code bellow

    <?php

    /**
    * Defining the class namespace
    * 
    * This is made of the vendor name and the package name.
    */
    namespace ChicoFreitas\GettingStarted;

    /**
    * Class GettingStarted
    */
    class GettingStarted {

        /**
        * @return String
        */
        public function printSomeMessage()
        {
            return 'Doing something in my getting started package!';
        }
    }

## Seeing things working

In the root of the package-developer project, open the composer.json and add your package 
to the autoload section

    "autoload": {
        "psr-4": {
            "App\\": "app/",
            "Database\\Factories\\": "database/factories/",
            "Database\\Seeders\\": "database/seeders/",
            "ChicoFreitas\\GettingStarted\\" : "vendor/chicofreitas/getting-started-package/src/"
        }
    },

It'll instruct composer to load the vendor/chicofreitas/getting-started-package/src/ directory as the ChicoFreitas/GettingStarted namespace.

Until now, our package is not into account inside our autoload files. So run the command in the root of 
the Laravel project

    $ composer dump-autoload

to solve this issue. Then, go to the *routes/web.php* file and add the following

    Route::get('/hello', function(ChicoFreitas\GettingStarted\GettingStarted $gt){
        return $gt->printSomeMessage();
    });

While in the root project directory, run

    $ php artisan serve

and access the URL *http://127.0.0.1:8000/hello*

## Creating the Service Provider

*src/Providers/GettingStartedProvider.php*

    <?php

    namespace ChicoFreitas\GettingStarted\Providers;

    use Illuminate\Support\ServiceProvider;

    class GettingStartedProvider extends ServiceProvider
    {
        /**
        * Bootstrap services.
        * 
        * @return void
        */
        public function boot()
        {
            # code...
        }
    }

Registering the service provider inside *config/app.php*

    #
    'providers' => ServiceProvider::defaultProviders()->merge([
        ...
        ChicoFreitas\GettingStarted\Providers\GettingStartedProvider::class,
    ])->toArray(),
    #

## Saving the package at GitHub

At this point, this package is at your 0.3.0 version since we made commits in each development step. 
Now we will commit out 0.3.0 branch and create a release in our repo.