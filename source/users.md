---
layout: default
title: Users
generator: pagination
pagination:
    max_per_page: 3
use:
    - posts
---
## Installation

The recommended method of installation is [through composer](http://getcomposer.org). On your local filesystem, create a new directory. Within that directory, create a `composer.json` file by executing the following command:

```bash
composer require "phergie/phergie-irc-bot-react"
```

From this created directory, [install composer](https://getcomposer.org/download/) and run `php composer.phar install`. This will install Phergie and all needed core dependencies.

## Configuration

A Phergie installation needs a minimum of two things to be particularly useful: connections to servers and plugins to interact with those servers. Both of these are specified using a Phergie configuration file, which is simply a PHP file that returns an associative array. Here's [an example configuration file](https://github.com/phergie/phergie-irc-bot-react/blob/master/config.sample.php). Let's examine its parts.

### Connections

Connecting to an IRC server requires at minimum the hostname of the server and a nickname, username, and real name for the bot on that server. Connections are represented by instances of classes that implement [`ConnectionInterface`](https://github.com/phergie/phergie-irc-connection/blob/master/src/ConnectionInterface.php) such as [`Connection`](https://github.com/phergie/phergie-irc-connection/blob/master/src/Connection.php), which handily supports configuration via its constructor.

Within the array returned by the Phergie configuration file, there must be a `'connections'` key that references an array of one or more connection instances.

```php
use Phergie\Irc\Connection;
return array(
  'connections' => array(
    new Connection(array(
      'serverHostname' => 'irc.freenode.net',
      'username' => 'Elazar',
      'realname' => 'Matthew Turland',
      'nickname' => 'Phergie3'
    )),
    new Connection(array(
      'serverHostname' => 'irc.efnet.org',
      'username' => 'MTurland',
      'realname' => 'Matthew Turland',
      'nickname' => 'Matt'
    ))
  )
);
```

#### SSL Connections

Connecting to an IRC server with SSL is possible. The `'serverPort'` key needs to be specified as well as an `'options'` key of `'transport' => 'ssl'` added to the `'connections'` key.

```php
use Phergie\Irc\Connection;
return array(
  'connections' => array(
    new Connection(array(
      'serverHostname' => 'irc.freenode.net',
      'serverPort' => 6697,
      'username' => 'Elazar',
      'realname' => 'Matthew Turland',
      'nickname' => 'Phergie3',
      'options' => array(
        'transport' => 'ssl'
      )
    ))
  )
);
```

##### Known Issues with SSL Connections

There is a known issue regarding timeouts with SSL connections. If the bot appears to startup and then do nothing, double check the serverHostname and serverPort configuration values as provided by the IRC network you are trying to connect to. These values must be absolutely correct! There is a bug report for this issue at [`reactphp/socket-client #28`](https://github.com/reactphp/socket-client/issues/28)

### Plugins

Plugins are PHP classes used to receive and/or send events between the bot and the servers it connects to. To use a plugin, it must be installed and the bot must be configured to use it. As with the bot itself, the recommended method of installing plugins is through composer.

One commonly used plugin is the [AutoJoin plugin](https://github.com/phergie/phergie-irc-plugin-react-autojoin), which joins the channels you specify on connection to a server. Let's walk through adding a plugin to a Phergie installation using the AutoJoin plugin as an example.

To install the plugin, execute the following command:

```bash
composer require "phergie/phergie-irc-plugin-react-autojoin"
```

Now that the plugin is installed, it needs to be added to the Phergie configuration file. Within the array returned by that file, in addition to the `'connections'` key added in the last section, there can also be a `'plugins'` key that references an array of one or more plugin instances.

```php
return array(
  // ...
  'plugins' => array(
    new \Phergie\Irc\Plugin\React\AutoJoin\Plugin(['channels' => '#phergie'])
  )
);
```

This configuration will have the bot join the #phergie channel once it connects to a server.

For more plugins to install, check out the [Plugins](/plugins) page.

## Usage

Now that connection information has been added to the configuration file, a plugin has been installed, and that plugin has been enabled in the configuration file, the bot is ready to be run. To do this, from the created directory, execute the bot runner with the path to the configuration file.

```
./vendor/bin/phergie path/to/config.php
```

If a configuration file path is not specified, the bot runner will check for a file called `config.php` in the current working directory by default.

## Logging

By default, Phergie uses [Monolog](http://github.com/Seldaek/monolog) for logging. More specifically, it uses a Monolog logger configured with a `DEBUG` log level that sends messages to `stderr`. This is the cause of the console output that results from running the bot, which emits all its IRC interactions as `DEBUG`-level messages.

Phergie supports use of any logging library that implements the [PSR-3 standard](http://www.php-fig.org/psr/psr-3/). To use a custom logger, within the array returned by the configuration file, add a `'logger'` key that references the logger instance to use it.

```php
use My\Logger;
return array(
  // ...
  'logger' => new My\Logger
);
```

## Troubleshooting

If you encounter an issue, check out the [#phergie channel](irc://irc.freenode.net/phergie) on the Freenode IRC network or [file an issue on Github](https://github.com/phergie/phergie-irc-bot-react/issues).
