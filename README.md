# [laravel-assets](http://roumen.it/projects/laravel-assets) package

[![Latest Stable Version](https://poser.pugx.org/roumen/asset/version.png)](https://packagist.org/packages/roumen/asset) [![Total Downloads](https://poser.pugx.org/roumen/asset/d/total.png)](https://packagist.org/packages/roumen/asset) [![Build Status](https://travis-ci.org/RoumenDamianoff/laravel-assets.png?branch=master)](https://travis-ci.org/RoumenDamianoff/laravel-assets) [![License](https://poser.pugx.org/roumen/asset/license.png)](https://packagist.org/packages/roumen/asset)

A simple assets manager for Laravel 4.


## Installation

Add the following to your `composer.json` file :

```json
"roumen/asset": "dev-master"
```

Then register this service provider with Laravel :

```php
'Roumen\Asset\AssetServiceProvider',
```

and add class alias for easy usage
```php
'Asset' => 'Roumen\Asset\Asset',
```

Don't forget to use ``composer update`` and ``composer dump-autoload`` when is needed!

## Example

```php
// adds css asset (accepts string or array input)
Asset::add('styles/default.css');

// adds less asset (accepts string or array input)
Asset::add('styles/default.less');

// adds js asset (accepts string or array input)
Asset::add('js/some.js');

// adds js asset to the 'footer' array
Asset::add('js/some.js', 'footer');

// adds script to 'ready' array (scripts are loaded in $(document).ready() function)
$script1 = '$("#hello").html("Hello World!")';
Asset::addScript($script1, 'ready');

// loads css assets (place this in your master layout before close head tag) wrapped in <link> tags
Asset::css();

// loads css assets (place this in your master layout before close head tag) not wrapped in <link> tags and joined using a custom separator
Asset::cssRaw('\n');

// loads js assets for your header and footer wrapped in <script> tags
Asset::js();
Asset::js('header');

// loads js assets for your header and footer not wrapped in <script> tags and joined using a custom separator
Asset::jsRaw('\n');

// loads all scripts for your header, footer or for your $(document).ready() function
Asset::scripts('header');
Asset::scripts('footer');
Asset::scripts('ready');

// adds an asset as first in its own array
Asset::addFirst('js/toBeLoadedFirst.js');

// adds an asset before another in its own array
Asset::addBefore('js/toBeLoadedBefore.js','js/ThisElement.js');

// adds an asset after another in its own array
Asset::addAfter('js/toBeLoadedAfter.js','js/ThisElement.js');

// set a domain name for your local assets in production environment
// must end with slash ('/') and point to your files (domain alias, cdn etc.)
// must be placed before Asset::css() or Asset::js() statements
Asset::setDomain('http://static.mydomain.ltd/');

// set a cache buster file that contains asset hashes
// it should be a JSON file should be structured as follows: {"filename.js":"hash","filename.css":"hash",...}
// if provided, your assets URLs will be appended with "?hash" string (e.g., http://example.com/filename.css?529e54acf891ccc6592f115afa1cc077)
Asset::setCachebuster(public_path() . '/build/assets.json');
```

## Example layout structure

```php
<!DOCTYPE html>
<html>
	<head>
		<title>Title</title>
		<!-- css files -->
		{{ Asset::css() }}
		<!-- less files -->
		{{ Asset::less() }}
		<!-- css styles -->
		{{ Asset::styles() }}
		<!-- js files (header) -->
		{{ Asset::js('header') }}
		<!-- js scripts (header) -->
		{{ Asset::scripts('header') }}
	</head>
	<body>
		<!-- content of nested view -->
		{{ $content }}

		<!-- js files -->
		{{ Asset::js() }}
		<!-- js scripts -->
		{{ Asset::scripts('footer') }}
		<!-- jquery scripts -->
		{{ Asset::scripts('ready') }}
	</body>
</html>
```
## Changelog

v2.3.11 - Added addBefore() and addAfter() methods

v2.3.6 - Added LESS support

v2.3.3 - Added setCachebuster(), jsRaw() and cssRaw() methods

v2.0 - Added setPrefix() method

v1.9 - Bug fixes, improvements to styles(), js() and css()

v1.5 - You can now use your own asset groups

```php
// add assets to 'foobar' group
{{ Asset:add('foo.js','foobar') }}
{{ Asset:add('bar.js','foobar') }}
{{ Asset:addScript($script,'foobar') }}

// load assets from 'foobar' group
{{ Asset:js('foobar') }}
{{ Asset:scripts('foobar') }}
```