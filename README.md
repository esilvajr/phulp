<p align="center"><img src="https://raw.githubusercontent.com/reisraff/phulp/master/phulp.png" alt="phulp" /></p>

<p align="center">The task manager for php</p>

[![Latest Stable Version](https://poser.pugx.org/reisraff/phulp/v/stable)](https://packagist.org/packages/reisraff/phulp)
[![Total Downloads](https://poser.pugx.org/reisraff/phulp/downloads)](https://packagist.org/packages/reisraff/phulp)
[![Latest Unstable Version](https://poser.pugx.org/reisraff/phulp/v/unstable)](https://packagist.org/packages/reisraff/phulp)
[![License](https://poser.pugx.org/reisraff/phulp/license)](https://packagist.org/packages/reisraff/phulp)
[![Build Status](https://travis-ci.org/reisraff/phulp.svg?branch=master)](https://travis-ci.org/reisraff/phulp)

### Why

Sometimes I need a tool like Gulp for my PHP projects, but I don't want to install `npm` only to install Gulp. I thought "I need something like Gulp, but in PHP". After a little research I found Phing, but it's not focused in minification and management for CSS/JS and related frontend stuff.

Well, I decided to write Phulp, the PHP port of Gulp! And a little curiosity: it's faster than Gulp.

**PS: I made benchs using PHP 7**

### Documentation

#### Usage

##### Install:

```bash
$ composer require --dev reisraff/phulp
```

##### Create your `Phulpfile`:

```php
<?php

// Define the default task
$phulp->task('default', function ($phulp) {
    $return = $phulp->exec(
        [
            'command' => 'echo $MSG',
            'env' => [
                'MSG' => 'Sync-command'
            ],
            'cwd' => '/tmp'
        ]
    );

    // $return['exit_code']
    // $return['output']

    $phulp->exec(
        [
            'command' => 'echo $MSG',
            'env' => [
                'MSG' => 'Assync-command'
            ],
            'cwd' => '/tmp'
        ],
        true, // defines async
        function ($exitCode, $output) {
            // do something
        }
    );

    $phulp->start(['clean']);

    // Define the source folder
    $phulp->src(['src/'], '/php$/', false)
        ->pipe($phulp->iterate(function ($distFile) {
            \Phulp\Output::out(\Phulp\Output::colorize($distFile->getName(), 'blue'));
        }))
        ->pipe($phulp->dest('dist/'));
});

// Define the clean task
$phulp->task('clean', function ($phulp) {
    $phulp->src(['dist/'])
        ->pipe($phulp->clean());
});

// Define the watch task
$phulp->task('watch', function ($phulp) {
    // Phulp will watch 'src' folder
    $phulp->watch(
        $phulp->src(['src/'], '/php$/', false),
        function ($phulp) {
            $phulp->start(['default']);
        }
    );
});
```

##### Run:

_If you have not configured the bin-dir:_

```bash
$ vendor/bin/phulp --help
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

The "Issues" page from this repository is being used for TO-DO management.

## Credits

[@reisraff](http://www.twitter.com/reisraff)
