# Phulp

[![Latest Stable Version](https://poser.pugx.org/reisraff/phulp/v/stable)](https://packagist.org/packages/reisraff/phulp)
[![Total Downloads](https://poser.pugx.org/reisraff/phulp/downloads)](https://packagist.org/packages/reisraff/phulp)
[![Latest Unstable Version](https://poser.pugx.org/reisraff/phulp/v/unstable)](https://packagist.org/packages/reisraff/phulp)
[![License](https://poser.pugx.org/reisraff/phulp/license)](https://packagist.org/packages/reisraff/phulp)
[![Build Status](https://travis-ci.org/reisraff/phulp.svg?branch=master)](https://travis-ci.org/reisraff/phulp)

The task manager for php

### Why

Sometimes I need a tool like Gulp for my PHP projects, but I don't want to install `npm` only to install Gulp. I thought "I need something like Gulp, but in PHP". After a little research I found Phing, but it's not focused in minification and management for CSS/JS and related frontend stuff.

Well, I decided to write Phulp, the PHP port of Gulp! And a little curiosity: it's faster than Gulp.

**PS: I made benchs using PHP 7**

### Documentation

#### Usage

##### Install:

```bash
$ composer require reisraff/phulp:~0.0.3
```

##### Create your `PhulpFile.php`:

```php
<?php

use Phulp\Phulp;

Phulp::task('default', function () {
    Phulp::start(['clean']);

    Phulp::src(['src/'], '/php$/', false)
        // ->pipe(\Phulp\PipeInterface)
        ->pipe(Phulp::iterate(function ($distFile) {
            \Phulp\Output::out($distFile->getName(), 'blue');
        }))
        ->pipe(Phulp::dest('dist/'));
});

Phulp::task('clean', function () {
    Phulp::src(['dist/'])
        ->pipe(Phulp::clean());
});

Phulp::task('watch', function () {
    Phulp::watch(
        Phulp::src(['src/'], '/php$/', false),
        ['default']
    );
});

```

##### Run:

_If you have not configured the bin-dir:_

```bash
$ vendor/bin/phulp # Will run the `default` task
$ vendor/bin/phulp watch # Will run the `watch` task
```

##### The full documentation:

[Docs](https://github.com/reisraff/phulp/blob/master/DOCUMENTATION.md)

### Contributors Guide

#### Clone

```bash
$ git clone git@github.com:reisraff/phulp.git
$ cd phulp
$ composer install
```

#### Tests

_First install the dependencies, and after you can run:_

```bash
$ bin/phing
```

### TODO

The "Issues" page from this repository is being used for TO-DO management, just search for the "to-do" tag.

## Credits

[@reisraff](http://www.twitter.com/reisraff)
