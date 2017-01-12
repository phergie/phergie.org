---
layout: default
generator: pagination
pagination:
    max_per_page: 3
use:
    - posts
---
* [Developing Plugins](#developing-plugins)
  * [Why Plugins?](#why-plugins)
  * [What You Need](#what-you-need)
  * [Plugins Defined](#plugins-defined)
  * [Supported Events](#supported-events)
  * [Responding to Events](#responding-to-events)
  * [Asynchronous and Timed Events](#asynchronous-and-timed-events)
  * [Custom Events](#custom-events)
  * [Logging](#logging)
  * [Installation](#installation)
* [Bot Customization](#bot-customization)
  * [Dependencies](#dependencies)
  * [Plugin Processors](#plugin-processors)
* [Core Development](#core-development)
  * [Running Tests](#running-tests)

## Developing Plugins

### Why Plugins?

Plugins should be written when you either want to intercept events received from the IRC server and respond to them in some way or when you want to asynchronously send events to the IRC server. If the problem you're trying to solve with your code doesn't involve interacting with the IRC server in some way, it's probably better to structure it as a library for other plugins to use.

### What You Need

The `phergie-scaffold` tool was created to automate the creation of files commonly included by plugin repositories. See [its GitHub repository](https://github.com/phergie/phergie-irc-plugin-react-scaffold) for more information on installing and using it.

Alternatively, create a directory containing a `composer.json` file with the contents shown below, then run `composer install`. This will install the bot and all its dependencies. See [Composer documentation](https://getcomposer.org/doc/00-intro.md) for further information.

```json
require: {
    "phergie/phergie-irc-bot-react": "^2.0"
}
```

You can also install Phergia via the Composer command-line interface:


```shell
composer require phergie/phergie-irc-bot-react ^2.0
```

As you're reviewing this page, the [API documentation](http://phergie.github.io/api/namespaces/Phergie.Irc.html) may come in handy.

[top](#)

## Plugins Defined

Plugins are classes that implement the
[`PluginInterface`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/PluginInterface.php)
interface. This interface contains a single method, `getSubscribedEvents()`,
which returns an associative array in which the keys are event names and the
values are either valid callbacks or names of instance methods in the plugin 
class to handle those events (i.e. `'methodName'` as a convenient shorthand for `array($this, 'methodName')`).

```php
use Phergie\Irc\Event\EventInterface as Event;
use Phergie\Irc\Bot\React\EventQueueInterface as Queue;
use Phergie\Irc\Bot\React\PluginInterface;

class ExamplePlugin implements PluginInterface
{
    public function getSubscribedEvents()
    {
        return array(
            'irc.received.privmsg' => 'onPrivmsg'
        );
    }

    public function onPrivmsg(Event $event, Queue $queue)
    {
        // ...
    }
}
```

In the above example, `'irc.received.privmsg'` is an event name and
`'onPrivmsg'` is the name of a method in the `ExamplePlugin` class to handle
that event.

[top](#)

### Supported Events

#### IRC Events

Plugins can listen for [connection and IRC events](https://github.com/phergie/phergie-irc-client-react#client-events) supported by the underlying client as well as the events shown below.

* **irc.received.each** - occurs when any type of event is received from a server
* **irc.received.TYPE** - occurs when an event of type `TYPE` (e.g. `privmsg`) is received from a server
* **irc.sending.all** - occurs after an event has been processed by all plugins, at which point all synchronous responses have been enqueued
* **irc.sending.each** - occurs before any type of event is sent to a server
* **irc.sending.TYPE** - occurs before an event of type `TYPE` (e.g. `privmsg`) is sent to a server
* **irc.sent.each** - occurs after any type of event is sent to a server
* **irc.sent.TYPE** - occurs when an event of type `TYPE` (e.g. `privmsg`) is sent to a server

Valid values for `TYPE` are lowercase strings that include the following:

* [IRC client commands](http://tools.ietf.org/html/rfc1459.html#section-4.2), e.g. `'privmsg'` or `'notice'`;
* [IRC server reply names](http://tools.ietf.org/html/rfc1459.html#section-6), e.g. `'err_nosuchnick'` or `'rpl_listend'`; and
* [CTCP client commands](http://www.irchelp.org/irchelp/rfc/ctcpspec.html) prefixed with the string `'ctcp.'`, e.g. `'ctcp.action'`.

Note that the bot handles sending [connection registration events](http://tools.ietf.org/html/rfc1459.html#section-4.1), so there's no need for a plugin to do so, but plugins can still subscribe to these events. For example, to have a plugin execute a callback once per connection, have it subscribe to the `irc.event.user` event.

IRC event handler methods typically accept two parameters:

* `$event`, an object that contains data about the event and implements [`EventInterface`](Even://github.com/phergie/phergie-irc-event/blob/master/src/EventInterface.php)
or subinterfaces of it such as
[`UserEventInterface`](https://github.com/phergie/phergie-irc-event/blob/master/src/UserEventInterface.php),
[`ServerEventInterface`](https://github.com/phergie/phergie-irc-event/blob/master/src/ServerEventInterface.php),
and [`CtcpEventInterface`](https://github.com/phergie/phergie-irc-event/blob/master/src/CtcpEventInterface.php);
and
* `$queue`, an object that sends events back to the server that sent the
original event and implements
[`EventQueueInterface`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/EventQueueInterface.php), a subinterface of [`GeneratorInterface`](https://github.com/phergie/phergie-irc-generator/blob/master/src/GeneratorInterface.php) where most of its methods are found.

One exception to this is the `'irc.sending.all'` event, which takes only the `$queue` parameter.

[top](#)

### Responding to Events

Plugins typically respond to events by sending a command to the server, such as sending a message back to the channel in which an event occurred. This is done using the `$queue` parameter passed to the event handler method as described at the end of the previous section. Here's an example:

```php
use Phergie\Irc\Event\UserEventInterface as Event;
use Phergie\Irc\Bot\React\EventQueueInterface as Queue;
use Phergie\Irc\Bot\React\PluginInterface;

class GreeterPlugin implements PluginInterface
{
    public function getSubscribedEvents()
    {
        return array(
            'irc.received.join' => 'onJoin',
        );
    }

    public function onJoin(Event $event, Queue $queue)
    {
        $channel = $event->getSource();
        $message = 'Welcome to the channel ' . $event->getNick() . '!';
        $queue->ircPrivmsg($channel, $message);
    }
}
```

This plugin has subscribed to the `'irc.received.join'` event, which occurs whenever a user joins a channel in which the bot is present. When this event occurs, the plugin's `onJoin()` method is invoked. 

This method receives two parameters:
* `$event`, an object that implements [`UserEventInterface`](https://github.com/phergie/phergie-irc-event/blob/master/src/UserEventInterface.php)
* `$queue`, an object that implements [`EventQueueInterface`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/EventQueueInterface.php)

To get the channel in which the original event occurred, `onJoin()` invokes `$event->getSource()`. If the event occurs in a channel, `getSource()` will return the name of that channel. If the event occurs as a direct interaction between another user and the bot, `getSource()` will instead return the nickname of that user.

`'irc.received.join'` events always occur in channels. To address a user who joins a channel, `onJoin()` needs that user's nickname, which it obtains using `$event->getNick()`. This method always returns the nickname of the event's originating user, as opposed to `$event->getSource()` which returns either that nickname or the name of a channel depending on the context of the event.

Finally, `onJoin()` invokes `$queue->ircPrivmsg()` to send the constructed message back to the channel.

[top](#)

### Asynchronous and Timed Events

To have a plugin execute a method on a short (normally sub-second) interval, have it subscribe to the `'irc.tick'` event. To have more control over the interval, your plugin must access the [event loop](https://github.com/reactphp/event-loop#eventloop-component) of the bot's client. For this to happen, the plugin must implement [`LoopAwareInterface`](https://github.com/phergie/phergie-irc-client-react/blob/master/src/LoopAwareInterface.php).

```php
use Phergie\Irc\Client\React\LoopAwareInterface;
use Phergie\Irc\Bot\React\PluginInterface;
use React\EventLoop\LoopInterface;

class Plugin implements PluginInterface, LoopAwareInterface
{
    public function setLoop(LoopInterface $loop)
    {
        $loop->addPeriodicTimer(
            5,                            // Every 5 seconds...
            array($this, 'timedCallback') // ... execute this callback
        );
    }

    public function timedCallback()
    {
        // ...
    }
}
```

Sending events to a server requires access to an event queue for the connection to that server (i.e. each connection has its own event queue). To do this, plugins must first access the connection object, then use that together with the event queue factory to access that connection's event queue object. Here's an example of a plugin that does this:

```php
use Phergie\Irc\Client\React\LoopAwareInterface;
use Phergie\Irc\ConnectionInterface;
use Phergie\Irc\Bot\React\AbstractPlugin;
use React\EventLoop\LoopInterface;
use React\EventLoop\Timer\TimerInterface;

class Plugin extends AbstractPlugin implements LoopAwareInterface
{
    // Getting connection objects:

    protected $connections;
  
    public function getSubscribedEvents()
    {
        return [
            'connect.after.each' => 'addConnection',
        ];
    }
  
    public function addConnection(ConnectionInterface $connection)
    {
        $this->getConnections()->attach($connection);
    }

    public function getConnections()
    {
        if (!$this->connections) {
            $this->connections = new \SplObjectStorage;
        }
        return $this->connections;
    }

    // Setting up the asynchronous callback that uses the event queue:

    public function setLoop(LoopInterface $loop)
    {
        $loop->addPeriodicTimer(30, array($this, 'myTimerCallback'));
    }

    public function myTimerCallback(TimerInterface $timer)
    {
        $factory = $this->getEventQueueFactory();
        foreach ($this->getConnections() as $connection) {
            $queue = $factory->getEventQueue($connection);
            // Use the queue to do whatever you like
        }
    }
}
```

[top](#)

### Custom Events

In addition to the core supported events that plugins can send and receive, they can also communicate with each other by sending and receiving custom events. To do this, they must implement [`EventEmitterAwareInterface`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/EventEmitterAwareInterface.php). Though this is relatively trivial to do, as the interface only contains a single `setEventEmitter()` method, a shortcut to doing so is to extend [`AbstractPlugin`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/AbstractPlugin.php), which provides an implementation of the interface.

Once obtained via `setEventEmitter()`, the event emitter object (which implements [`EventEmitterInterface`](https://github.com/igorw/evenement/blob/master/src/Evenement/EventEmitterInterface.php)) has an `emit()` method that can be used to emit an event that any plugins subscribed to it will receive.

```php
$eventEmitter->emit('namespace.event.subevent', $parameters);
```

Event names are specified as strings. They are conventionally namespaced to avoid naming collisions with other plugins, with name segments delimited using periods.

`$parameters` is an array of parameter values received by event handler methods of subscribed plugins.

[top](#)

### Logging

Plugins can gain access to the same [logger instance used by core logic](Usage#logging) by implementing [`LoggerAwareInterface`](https://github.com/php-fig/log/blob/master/Psr/Log/LoggerAwareInterface.php). Though this is relatively trivial to do, as the interface only contains a single `setLogger()` method, a shortcut to doing so is to extend [`AbstractPlugin`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/AbstractPlugin.php), which provides an implementation of the interface.

Once obtained via `setLogger()`, the logger object can be used to log whatever events may be relevant to monitoring or debugging the plugin. In particular, one noteworthy shortcoming of Phergie's use of event callbacks is that there's no way to accurately attribute events sent by plugins in log messages (for debugging purposes) that isn't extremely hacky. As such, logging a message when a plugin sends an event is an advisable practice.

[top](#)

### Installation

Plugins are conventionally installed [using composer](/users#plugins). To support this, a [`composer.json` file](https://getcomposer.org/doc/04-schema.md) should be included with the plugin source code that provides information about the plugin and any [dependencies](https://getcomposer.org/doc/04-schema.md#package-links) it has on other plugins or libraries. See the `composer.json` files included with [existing plugins](/plugins) for examples.

[top](#)

## Bot Customization

### Dependencies

The bot is represented by the [`Bot`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/Bot.php) class, which is used by the [bot runner](https://github.com/phergie/phergie-irc-bot-react/blob/master/bin/phergie). In addition to the [logger](/users#logging), this class supports replacing other dependencies via configuration.

* `'client'` - an object that implements [`ClientInterface`](https://github.com/phergie/phergie-irc-client-react/blob/master/src/ClientInterface.php) and is used for low-level IRC client-server interactions and emission of custom events, e.g. [`Client`](https://github.com/phergie/phergie-irc-client-react/blob/master/src/Client.php)
* `'parser'` - an object that implements [`ParserInterface`](https://github.com/phergie/phergie-irc-parser/blob/master/src/ParserInterface.php) and is used to parse data from streams of IRC interactions, e.g. [`Parser`](https://github.com/phergie/phergie-irc-parser/blob/master/src/Parser.php)
* `'converter'` - an object that implements [`ParserConverterInterface`](https://github.com/phergie/phergie-irc-event/blob/master/src/EventInterface.php) and is used to convert parsed IRC interaction data into event objects, e.g. [`ParserConverter`](https://github.com/phergie/phergie-irc-event/blob/master/src/ParserConverter.php)
* `'eventQueueFactory'` - an object that implements [`EventQueueFactoryInterface`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/EventQueueFactoryInterface.php) and is used to obtain connection-specific objects that implement [`EventQueueInterface`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/EventQueueInterface.php) -- such as [`EventQueue`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/EventQueue.php) -- for sending events to IRC servers, e.g. [`EventQueueFactory`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/EventQueueFactory.php)

Here's an example configuration file that implements overrides of these dependencies:

```php
return array(
  'connections' => array(
    // ...
  ),
  'plugins' => array(
    // ...
  ),
  'client' => new My\Client,
  'parser' => new My\Parser,
  'converter' => new My\Converter,
  'eventQueueFactory' => new My\EventQueueFactory
);
```

[top](#)

### Plugin Processors

Plugins sometimes require some common form of dependency injection or other modification after they're loaded. This is handled by plugin processors, which can be set via the `'pluginProcessors'` configuration key as an array of objects implementing [`PluginProcessorInterface`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/PluginProcessor/PluginProcessorInterface.php). If no value is set, by default, the bot will use these plugin processors:

* [`EventEmitterInjector`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/PluginProcessor/EventEmitterInjector.php) injects the plugin with the [bot's client](#dependencies) if the plugin implements [`EventEmitterAwareInterface`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/EventEmitterAwareInterface.php). If so, the plugin can use the client to emit events to which other plugins can subscribe.
* [`EventQueueFactoryInjector`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/PluginProcessor/EventQueueFactoryInjector.php) injects the plugin with the [bot's event queue factory](#dependencies) if the plugin implements [`EventQueueFactoryAwareInterface`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/EventQueueFactoryAwareInterface.php). If so, the plugin can access event queues for all connections, which is useful when emitting [asynchronous and timed events](Development#asynchronous-and-timed-events).
* [`ClientInjector`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/PluginProcessor/ClientInjector.php) injects the plugin with the [bot's client](#dependencies) if the plugin implements [`ClientAwareInterface`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/ClientAwareInterface.php) for contexts in which the client is not being used solely as an event emitter.
* [`LoggerInjector`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/PluginProcessor/LoggerInjector.php) injects the plugin with the same [logger used by the bot](/users#logging) if the plugin implements [`LoggerAwareInterface`](https://github.com/php-fig/log/blob/master/Psr/Log/LoggerAwareInterface.php).
* [`LoopInjector`](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/PluginProcessor/LoopInjector.php) injects the plugin with the [event loop](https://github.com/reactphp/event-loop#eventloop-component) used by the bot's client if the client implements [`LoopAccessorInterface`](https://github.com/phergie/phergie-irc-client-react/blob/master/src/LoopAccessorInterface.php) and the plugin implements [`LoopAwareInterface`](https://github.com/phergie/phergie-irc-client-react/blob/master/src/LoopAwareInterface.php). If so, the plugin can use the event loop to execute stream and timed operations.

[top](#)

## Core Development

### Running Tests

To run the phergie-irc-bot-react unit test suite:

```
curl -s https://getcomposer.org/installer | php
php composer.phar install
php composer.phar require phpunit/phpunit
cd tests
../vendor/bin/phpunit
```

[top](#)
