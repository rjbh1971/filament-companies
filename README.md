![gif (1)](https://user-images.githubusercontent.com/104294090/221399175-add7c34b-4887-49b7-9061-6781f6391409.gif)
<p align="center">
    <a href="https://filamentadmin.com/docs/2.x/admin/installation">
        <img alt="FILAMENT 8.x" src="https://img.shields.io/badge/FILAMENT-2.x-EBB304?style=for-the-badge">
    </a>
    <a href="https://packagist.org/packages/andrewdwallo/filament-companies">
        <img alt="Packagist" src="https://img.shields.io/packagist/v/andrewdwallo/filament-companies.svg?style=for-the-badge&logo=packagist">
    </a>
    <a href="https://packagist.org/packages/andrewdwallo/filament-companies">
        <img alt="Downloads" src="https://img.shields.io/packagist/dt/andrewdwallo/filament-companies?color=red&style=for-the-badge" >
    </a>
</p>

<hr style="background-color: #ebb304">

# Filament Companies

A Complete Authentication System Kit based on Companies built for Filament:
- :fire: **Authentication - Fortify** 
- :fire: **Socialite (Optional)**
- :fire: **Terms & Privacy Policy**
- :fire: **Password Reset via Email**
- :fire: **Personal Profile Management**
- :fire: **Two-Factor Authentication (2FA)**
- :fire: **Browser Session Management**
- :fire: **Sanctum API**
- :fire: **Company Management**
- :fire: **Employee Invitations via Email**
- :fire: **Roles & Permissions**
- :fire: **And More to Come!**

# Getting Started

### WARNING

* This plugin requires a fresh Filament project
* If you install this plugin into an existing Filament project you will get errors
* No modifications need to be made to the filament config files
* Everything is set once the plugin is installed

### Getting Set Up

* Create a fresh Laravel Project
* Configure your database
* Install the filament admin package

```
composer require filament/filament
```

# Installation

Install this package
```
composer require andrewdwallo/filament-companies
```

Use the following command to scaffold the app.
```
php artisan filament-companies:install filament --companies
```

In `config/fortify.php` replace the following:
```
'middleware' => ['web'],
```
to...
```
'middleware' => config('filament.middleware.base'),
```

### Finalizing Installation
```
php artisan migrate:fresh
```
```
npm run dev
```

# Usage

* Click Register on the Laravel Welcome Page
* After Registration you will be redirected to the Admin panel
* You can create companies by clicking the dropdown navigation in the Filament topbar.
* By clicking on your current company's settings in the topbar you can manage that current company.
* You may also switch your current company.
* You can also create API Tokens and manage your personal profile settings by clicking the filament user menu dropdown link.

This package is extensively "borrowed" from the work of Taylor Otwell, his contributors and the Laravel Jetstream package. You can get a full understanding of the capabilities by reviewing the Jetstream [Documentation](https://jetstream.laravel.com/2.x/introduction.html/).

If you want to change the filament path prefix to something such as "company", you may do so as you normally would in `config/filament.php`
```
/*
    |--------------------------------------------------------------------------
    | Filament Path
    |--------------------------------------------------------------------------
    |
    | The default is `admin` but you can change it to whatever works best and
    | doesn't conflict with the routing in your application.
    |
    */

    'path' => env('FILAMENT_PATH', 'company'),
```
> The Laravel Welcome Page, Fortify, etc.. will respect your changes.

### Socialite

By Default, the GitHub Provider will be enabled.

You may use any Provider that [Laravel Socialite](https://laravel.com/docs/10.x/socialite/) supports.

You may add or remove any Provider in `config/filament-company.php`

```
    /*
    |--------------------------------------------------------------------------
    | Socialite Providers
    |--------------------------------------------------------------------------
    |
    | Here you may specify the providers your application supports for OAuth.
    | Out of the box, FilamentCompanies provides support for all the OAuth
    | providers that are supported by Laravel Socialite.
    |
    */

    'providers' => [
        'twitter-oauth-2' => true,
        'bitbucket' => true,
        'facebook' => true,
        'linkedin' => true,
        'twitter' => true,
        'gitlab' => true,
        'google' => true,
        'github' => true,
    ],
```
> If any of these Provider's are undesired you do not have to set to false, you may completely remove them from the array.
> If Twitter is desired, you may only use either Twitter OAuth1 or Twitter OAuth2

In `config/services.php` pass your Provider's credentials in the providers array:
```
    'github' => [
        'client_id' => env('GITHUB_CLIENT_ID'),
        'client_secret' => env('GITHUB_CLIENT_SECRET'),
        'redirect' => 'https://filament.test/oauth/github/callback',
    ],
```
> The Provider's Redirect URI must look similar to the above

If Socialite is undesired for your Application you may comment out or completely remove the following from the `features` array:
```
/*
    |--------------------------------------------------------------------------
    | Features
    |--------------------------------------------------------------------------
    |
    | Some of Company's features are optional. You may disable the features
    | by removing them from this array. You're free to only remove some of
    | these features, or you can even remove all of these if you need to.
    |
    */

    'features' => [
        Features::termsAndPrivacyPolicy(),
        Features::profilePhotos(),
        Features::api(),
        Features::companies(['invitations' => true]),
        Features::accountDeletion(),
        // Features::socialite(['rememberSession' => true, 'providerAvatars' => true]),
    ],
```

The Socialite package is extensively "borrowed" from the work of Joel Butcher, his contributors and the Socialstream package. You can get a full understanding of the capabilities by reviewing the Socialstream [Documentation](https://docs.socialstream.dev/).

Note: The following examples are a visual representation of the features this package supports that were provided by the methods implemented in Laravel Jetstream. You may find all of the features as provided by the Laravel Jetstream package [here](https://jetstream.laravel.com/3.x/features/teams.html) in their documentation.

Information about a user's companies may be accessed via the methods provided by the `Wallo\FilamentCompanies\HasCompanies` trait. This trait is automatically applied to your application's `App\Models\User` model during installation. This trait provides a variety of helpful methods that allow you to inspect a user's companies or company:

```
// Access a user's currently selected company...
$user->currentCompany : Wallo\FilamentCompanies\Company

// Access all of the companies (including owned companies) that a user belongs to...
$user->allCompanies() : Illuminate\Support\Collection

// Access all of a user's owned companies...
$user->ownedCompanies : Illuminate\Database\Eloquent\Collection

// Access all of the companies that a user belongs to but does not own...
$user->companies : Illuminate\Database\Eloquent\Collection

// Access a user's "personal" company...
$user->personalCompany() : Wallo\FilamentCompanies\Company

// Determine if a user owns a given company...
$user->ownsCompany($company) : bool

// Determine if a user belongs to a given company...
$user->belongsToCompany($company) : bool

// Get the role that the user is assigned on the company...
$user->companyRole($company) : \Wallo\FilamentCompanies\Role

// Determine if the user has the given role on the given company...
$user->hasCompanyRole($company, 'admin') : bool

// Access an array of all permissions a user has for a given company...
$user->companyPermissions($company) : array

// Determine if a user has a given company permission...
$user->hasCompanyPermission($company, 'server:create') : bool
```

Example #1: Only allowing a certain company ID to see & visit a filament page, resource, etc...
```
protected static function shouldRegisterNavigation(): bool
{
    return Auth::user()->currentCompany->id === 3;
}

public function mount(): void
{
    abort_unless(Auth::user()->currentCompany->id === 3, 403);
}
```
> `Auth::user()` represents the current user of the application.


Example #2: Using the Current Company Name
```
protected static function shouldRegisterNavigation(): bool
{
    return Auth::user()->currentCompany->name === "Filament";
}

public function mount(): void
{
    abort_unless(Auth::user()->currentCompany->name === "Filament", 403);
}
```
> You can use collections of different companies and group them together, or you may use different ranges of values, and more.

### Note
* This package is supposed to be a Filament Context and is planning to be used as one in Filament V3.
* The default view after installation is not supposed to be the "Admin" Context, this would be the view that a "company user" would see.
* There are methods to support an "Admin" Context if wanted.

### Contributing
* Fork this repository to your GitHub account.
* Create a fresh Laravel & Filament Project.
* Clone your fork in your App's root directory.
* In the `/filament-companies` directory, create a branch for your fix, e.g. `fix/error-message`.

Install the plugin/package in your app's `composer.json` using the `dev` prefix followed by your branches name:
```json
{
    ...
    "require": {
        "andrewdwallo/filament-companies": "dev-fix/error-message",
    },
    "repositories": [
        {
            "type": "path",
            "url": "filament-companies/"
        }
    ],
    ...
}
```

* Now, run `composer update` and use the following command to scaffold the app.
```
php artisan filament-companies:install filament --companies
```
* You may continue by following the installation instructions above.
