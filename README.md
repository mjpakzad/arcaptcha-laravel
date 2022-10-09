<p align="center"><img src="resources/images/arcaptcha-logo.png" alt="ArCaptcha" width="500"></p>

# Laravel ArCaptcha Package

[![Latest Stable Version](http://poser.pugx.org/arcaptcha/arcaptcha-laravel/v)](https://packagist.org/packages/arcaptcha/arcaptcha-laravel)
[![Total Downloads](http://poser.pugx.org/arcaptcha/arcaptcha-laravel/downloads)](https://packagist.org/packages/arcaptcha/arcaptcha-laravel) [![Latest Unstable Version](http://poser.pugx.org/arcaptcha/arcaptcha-laravel/v/unstable)](https://packagist.org/packages/arcaptcha/arcaptcha-laravel)
[![License](http://poser.pugx.org/arcaptcha/arcaptcha-laravel/license)](https://packagist.org/packages/arcaptcha/arcaptcha-laravel)
[![PHP Version Require](http://poser.pugx.org/arcaptcha/arcaptcha-laravel/require/php)](https://packagist.org/packages/arcaptcha/arcaptcha-laravel)

Laravel Package for the ArCaptcha
This package supports `PHP 7.3+`.

For **PHP** integration you can use [mohammadv184/arcaptcha](https://github.com/mohammadv184/arcaptcha) package.

# List of contents

- [PHP ArCaptcha Library](#PHP-ArCaptcha-Library)
- [List of contents](#list-of-contents)
  - [Installation](#Installation)
  - [Configuration](#Configuration)
    - [Publish package](#publish-package)
    - [Set the environment](#set-the-environment)
    - [Customize error message](#customize-error-message)
  - [How to use](#how-to-use)
    - [Embed Script in Blade](#Embed-Script-in-Blade)
    - [Form setup](#Form-setup)
    - [Verify submitted data](#Verify-submitted-data)
  - [Credits](#credits)
  - [License](#license)

## Installation

You can install the package via composer:

```bash
composer require arcaptcha/arcaptcha-laravel
```

Laravel 5.5 (or greater) uses package auto-discovery, so doesn't require you to manually add the Service Provider, but if you
don't use auto-discovery ArCaptchaServiceProvider must be registered in config/app.php:

```php
'providers' => [
    ...
    Mohammadv184\ArCaptcha\Laravel\ArCaptchaServiceProvider::class,
];
```

You can use the facade for shorter code. Add ArCaptcha to your aliases:

```php
'aliases' => [
    ...
    'ArCaptcha' => Mohammadv184\ArCaptcha\Laravel\Facade\ArCaptcha::class,
];
```

## Configuration

### Publish package

Create config/arcaptcha.php configuration file using the following artisan command:

```php
php artisan vendor:publish --provider="Mohammadv184\ArCaptcha\Laravel\ArCaptchaServiceProvider"
```

### Set the environment

Open .env file and set `ARCAPTCHA_SITE_KEY` and `ARCAPTCHA_SECRET_KEY`:

```dotenv
# in your .env file
ARCAPTCHA_SITE_KEY=YOUR_API_SITE_KEY
ARCAPTCHA_SECRET_KEY=YOUR_API_SECRET_KEY
```

### Customize error message

Before starting please add the validation message to `resources/lang/[LANG]/validation.php` file

```php
return [
    ...
    'arcaptcha' => 'Hey!!! :attribute is wrong!',
];
```

## How to use

How to use ArCaptcha in Laravel.

### Embed Script in Blade

Insert `@arcaptchaScript` blade directive before closing `</head>` tag.

You can also use `ArCaptcha::getScript()`.

```html
<!DOCTYPE html>
<html>
  <head>
    ... @arcaptchaScript
  </head>
</html>
```

### Form setup

After you have to insert `@arcaptchaWidget` blade directive inside the form where you want to use the field `arcaptcha-token`.

You can also use `ArCaptcha::getWidget()`.

_Note : You can pass widget options into getWidget function or arcaptchaWidget directive like this : @arcaptchaWidget(['lang'=>'en'])_

_To see available options on widget see [here](https://docs.arcaptcha.ir/docs/configuration#arcaptcha-container-configuration)_

```html
<form>
  @csrf ... @arcaptchaWidget
  <!-- OR -->
  {!! ArCaptcha::getWidget() !!}
  <input type="submit" />
</form>
```

### Verify submitted data

Add `arcaptcha` to your rules

```php
$validator = Validator::make(request()->all(), [
        ...
        'arcaptcha-token' => 'arcaptcha',
    ]);

    // check if validator fails
    if($validator->fails()) {
        ...
        $errors = $validator->errors();
    }
```

## Invisible mode example

Just try to pass `size` and `callback` option to getWidget function. Make sure to define callback function in global scope:

```html
<form>
  {!! ArCaptcha::getWidget([ 'size'=>'invisible','callback'=>'callback']) !!}
  <input type="submit" />

  <script>
    function callback(token) {
      // Challenge is solved! Just submit the form!
    }
  </script>
</form>
```

## Credits

- [Mohammad Abbasi](https://github.com/mohammadv184)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE) for more information.
