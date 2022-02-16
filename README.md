# Watch changes in the file system using PHP
This package allows you to react to all kinds of changes in the file system.

Here's how you can run code when a new file gets added.

```php
use Spatie\Watcher\Watch;

Watch::path($directory)
    ->onFileCreated(function (string $newFilePath) {
        // do something...
    })
    ->start();
```

## Support

Php 7 | 8

## Installation

You can install the package via composer:

```bash
composer require spatie/file-system-watcher
```

In your project, you should have the JavaScript package [`chokidar`](https://github.com/paulmillr/chokidar) installed.
You can install it via npm

```bash
npm install chokidar
```

or Yarn

```bash
yarn add chokidar
```

## Usage

Here's how you can start watching a directory and get notified of any changes.

```php
use Spatie\Watcher\Watch;

Watch::path($directory)
    ->onAnyChange(function (string $type, string $path) {
        if ($type === Watch::EVENT_TYPE_FILE_CREATED) {
            echo "file {$path} was created";
        }
    })
    ->start();
```

You can pass as many directories as you like to `path`.

To start watching, call the `start` method.

### Detected the type of change

The `$type` parameter of the closure you pass to `onAnyChange` can contain one of these values:

- `Watcher::EVENT_TYPE_FILE_CREATED`: a file was created
- `Watcher::EVENT_TYPE_FILE_UPDATED`: a file was updated
- `Watcher::EVENT_TYPE_FILE_DELETED`: a file was deleted
- `Watcher::EVENT_TYPE_DIRECTORY_CREATED`: a directory was created
- `Watcher::EVENT_TYPE_DIRECTORY_DELETED`: a directory was deleted

### Listening for specific events

To handle file systems events of a certain type, you can make use of dedicated functions. Here's how you would listen
for file creations only.

```php
use Spatie\Watcher\Watch;

Watch::path($directory)
    ->onFileCreated(function (string $newFilePath) {
        // do something...
    });
```

These are the related available methods:

- `onFileCreated()`: accepts a closure that will get passed the new file path
- `onFileUpdated()`: accepts a closure that will get passed the updated file path
- `onFileDeleted()`: accepts a closure that will get passed the deleted file path
- `onDirectoryCreated()`: accepts a closure that will get passed the created directory path
- `onDirectoryDeleted()`: accepts a closure that will get passed the deleted directory path

### Watching multiple paths

You can pass multiple paths to the `paths` method.

```php
use Spatie\Watcher\Watch;

Watch::paths($directory, $anotherDirectory);
```

### Performing multiple tasks

You can call `onAnyChange`, 'onFileCreated', ... multiple times. All given closures will be performed

```php
use Spatie\Watcher\Watch;

Watch::path($directory)
    ->onFileCreated(function (string $newFilePath) {
        // do something on file creation...
    })
    ->onFileCreated(function (string $newFilePath) {
        // do something else on file creation...
    })
    ->onAnyChange(function (string $type, string $path) {
        // do something...
    })
    ->onAnyChange(function (string $type, string $path) {
        // do something else...
    })
    // ...
```

### Stopping the watcher gracefully

By default, the watcher will continue indefinitely when started. To gracefully stop the watcher, you can
call `shouldContinue` and pass it a closure. If the closure returns a falsy value, the watcher will stop. The given
closure will be executed every 0.5 second.

```php
use Spatie\Watcher\Watch;

Watch::path($directory)
    ->shouldContinue(function () {
        // return true or false
    })
    // ...
```

### Change the speed of watcher

By default, the changes are tracked every 0.5 seconds, however you could change that.

```php
use Spatie\Watcher\Watch;

Watch::path($directory)
    ->setIntervalTime(100) //microseconds
    // ...
```

## Testing

```bash
composer test
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Contributing

Please see [CONTRIBUTING](.github/CONTRIBUTING.md) for details.

## Security Vulnerabilities

Please review [our security policy](../../security/policy) on how to report security vulnerabilities.

## Credits

- [Freek Van der Herten](https://github.com/freekmurze)
- [All Contributors](../../contributors)

Parts of this package are inspired by [Laravel Octane](https://github.com/laravel/octane)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
