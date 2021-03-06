[<< previous](2-composer.md) | [next >>](4-http.md)

### Error Handler

An error handler allows you to customize what happens if your code results in an error.

A nice error page with a lot of information for debugging goes a long way during development. So the first package for your application will take care of that.

I like [filp/whoops](https://github.com/filp/whoops), so I will show how you can install that package for your project. If you prefer another package, feel free to install that one. This is the beauty of programming without a framework, you have total control over your project.

An alternative package would be: [PHP-Error](https://github.com/JosephLenton/PHP-Error)

To install a new package, open up your `composer.json` and add the package to the require part. It should now look like this:

```php
"require": {
    "php": ">=5.5.0",
    "filp/whoops": ">=1.1.2"
},
```

Now run `composer update` in your console and it will be installed.

But you can't use it yet. PHP won't know where to find the files for the classes. For this you will need an autoloader, ideally a [PSR-4](http://www.php-fig.org/psr/psr-4/) autoloader. Composer already takes care of this for you, so you only have to add a `require __DIR__ . '/../vendor/autoload.php';` to your `Bootstrap.php`.

**Important:** Never show any errors in your production environment. A stack trace or even just a simple error message can help someone to gain access to your system. Always show a user friendly error page instead and send an email to yourself, write to a log or something similar. So only you can see the errors in the production environment.

For development that does not make sense though -- you want a nice error page. The solution is to have an environment switch in your code. For now you can just set it to `development`.

Then after the error handler registration, throw an `Exception` to test if everything is working correctly. Your `Bootstrap.php` should now look similar to this:

```php
<?php

namespace Example;

require __DIR__ . '/../vendor/autoload.php';

error_reporting(E_ALL);

$environment = 'development';

/**
* Register the error handler
*/
$woops = new \Whoops\Run;
if ($environment !== 'production') {
    $woops->pushHandler(new \Whoops\Handler\PrettyPageHandler);
} else {
    $woops->pushHandler(function($e){
        echo 'Friendly error page and send an email to the developer';
    });
}
$woops->register();

throw new \Exception;

```

You should now see a error page with the line highlighted where you throw the exception. If not, go back and debug until you get it working. Now would also be a good time for another commit.

[<< previous](2-composer.md) | [next >>](4-http.md)

