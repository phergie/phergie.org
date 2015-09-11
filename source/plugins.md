---
layout: default
generator: pagination
pagination:
    max_per_page: 3
use:
    - posts
---
## Available Plugins

* **[AltNick](https://github.com/PSchwisow/phergie-irc-plugin-react-altnick)** - automatically switches to alternate nicks when primary is not available
* **[Audioscrobbler](https://github.com/chrismou/phergie-irc-plugin-react-audioscrobbler)** - returns the current or last played song for a user on last.fm or libre.fm
* **[AutoJoin](https://github.com/phergie/phergie-irc-plugin-react-autojoin)** - automatically joins channels when connecting to servers
* **[AutoRejoin](https://github.com/enebe-nb/phergie-irc-plugin-react-autorejoin)** - automatically re-joins channels on PART or KICK
* **[Bigstock](https://github.com/shutterstock/phergie-irc-plugin-react-bigstock)** - searches Bigstock API for an image that matches the query string
* **[Command](https://github.com/phergie/phergie-irc-plugin-react-command)** - parses custom commands within IRC messages and emits them as events for other plugins to consume
* **[CommandAlias](https://github.com/phergie/phergie-irc-plugin-react-commandalias)** - enables use of custom aliases for existing bot commands
* **[CommandHelp](https://github.com/phergie/phergie-irc-plugin-react-commandhelp)** - provides usage information for available bot commands to users
* **[Daddy](https://github.com/dstockto/phergie-irc-plugin-react-daddy)** - responds to phrases like "Who's your *?"
* **[Dice](https://github.com/chrismou/phergie-irc-plugin-react-dice)** - returns the total of a user specified number of dice rolls
* **[Dns](https://github.com/WyriHaximus/PhergieDns)** - enables other plugins to perform [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) lookups and provides a user command for the same
* **[EventFilter](https://github.com/phergie/phergie-irc-plugin-react-eventfilter)** - limits processing of incoming events by other plugins based on event metadata
* **[FeedTicker](https://github.com/phergie/phergie-irc-plugin-react-feedticker)** - syndicates data from feed items to channels or users
* **[Google](https://github.com/chrismou/phergie-irc-plugin-react-google)** - perform Google searches for the top result or estimated result count
* **[Http](https://github.com/WyriHaximus/PhergieHttp/)** - provides a basic asynchronous HTTP client for use by other plugins
* **[JoinPart](https://github.com/phergie/phergie-irc-plugin-react-joinpart)** - provides commands to instruct the bot to join and part channels
* **[MagicEightBall](https://github.com/dstockto/phergie-irc-plugin-react-magic-eightball)** - helps users make tough decisions
* **[NickServ](https://github.com/phergie/phergie-irc-plugin-react-nickserv)** - authenticates the bot with the NickServ agent
* **[OpsWorksDeployment](https://github.com/blopboard/phergie-irc-plugin-opsworksdeployment)** - provides deployment commands for [Amazon OpsWorks](http://aws.amazon.com/opsworks/)
* **[PHP](https://github.com/chrismou/phergie-irc-plugin-react-php)** - performs PHP function lookups
* **[Ping](https://github.com/Renegade334/phergie-irc-plugin-react-ping)** - closes the connection if it becomes unresponsive
* **[PingPong](https://github.com/chrismou/phergie-irc-plugin-react-pingpong)** - test bot responsiveness
* **[Pong](https://github.com/phergie/phergie-irc-plugin-react-pong)** - responds to server ping events
* **[Puppet](https://github.com/PSchwisow/phergie-irc-plugin-react-puppet)** - allows a user to effectively speak and act as the bot
* **[Quit](https://github.com/phergie/phergie-irc-plugin-react-quit)** - provides a command to instruct the bot to teminate a connection
* **[Seen](https://github.com/renegade334/phergie-irc-plugin-react-seen)** - provides a command to display a user's last channel activity
* **[Tell](https://github.com/enebe-nb/phergie-irc-plugin-react-tell)** - send messages to users next time they are seen
* **[TextFormatting](https://github.com/chrismou/php-irc-text-formatting)** - a third party library for adding color and styling to your plugin's text output
* **[TheFuckingWeather](https://github.com/phergie/phergie-irc-plugin-react-thefuckingweather)** - provides a command to retrieve weather information from [thefuckingweather.com](http://thefuckingweather.com)
* **[UserMode](https://github.com/phergie/phergie-irc-plugin-react-usermode)** - tracks channel-specific user modes and makes them accessible to other plugins
* **[Url](https://github.com/WyriHaximus/PhergieUrl)** - scans for and provides information about URLs
* **[UrlShorten](https://github.com/PSchwisow/phergie-irc-plugin-react-url-shorten)** - provides URL shortening services for Url plugin (and others)
* **[Weather](https://github.com/chrismou/phergie-irc-plugin-react-weather)** - perform current weather lookups for a given location
* **[Wikidata](https://github.com/addshore/phergie-irc-plugin-react-wikidata)** - performs lookups on Wikidata
* **[YouTube](https://github.com/phergie/phergie-irc-plugin-react-youtube)** - responds to [YouTube](https://www.youtube.com) URLs with video information

Edit this page to add yours! :)

## Plugins to Port

Below is a list of [plugins from Phergie v2](https://github.com/phergie/phergie/tree/master/Phergie/Plugin) and the status of porting them to Phergie v3.

Status Key:
* **Open** - free to be worked on
* In Progress - a contributor is currently working on a port
* Done - port is complete and available for use
* Deprecated - functionality has been moved to core or is not planned for porting
* Blocked - a dependency presently prevents progress on porting

| Plugin  | Status | Notes |
| ------- | ------ | ----- |
| [Acl](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Acl.php) | Done | [phergie/phergie-irc-plugin-react-eventfilter](https://github.com/phergie/phergie-irc-plugin-react-eventfilter) |
| [Ai](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Ai.php) | **Open** | |
| [AltNick](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/AltNick.php) | Done | [pschwisow/phergie-irc-plugin-react-altnick](https://github.com/PSchwisow/phergie-irc-plugin-react-altnick)|
| [AudioScrobbler](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/AudioScrobbler.php) | Done | [chrismou/phergie-irc-plugin-react-audioscrobbler](https://github.com/chrismou/phergie-irc-plugin-react-audioscrobbler) |
| [AutoJoin](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/AutoJoin.php) | Done | [phergie/phergie-irc-plugin-react-autojoin](https://github.com/phergie/phergie-irc-plugin-react-autojoin) |
| [Beer](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Beer.php) | Blocked | Needs Serve |
| [BeerScore](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/BeerScore.php) | **Open** | Should use [Http](#http) |
| [Cache](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Cache.php) | Deprecated | Use [existing third-party caching libraries](https://packagist.org/search/?q=cache) instead |
| [Caffeine](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Caffeine.php) | Blocked | Needs Serve |
| [Censor](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Censor.php) | Blocked | Needs [new API](http://wiki.cdyne.com/index.php/Profanity_Filter#Unsupported) |
| [Cocktail](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Cocktail.php) | Blocked | Needs Serve |
| [Command](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Command.php) | Done | [phergie/phergie-irc-plugin-react-command](https://github.com/phergie/phergie-irc-plugin-react-command) |
| [Cookie](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Cookie.php) | Blocked | Needs Serve |
| [Cron](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Cron.php) | Deprecated | Now [supported in core](https://github.com/phergie/phergie-irc-bot-react/wiki/Development#asynchronous-and-timed-events) |
| [Ctcp](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Ctcp.php) | **Open** | |
| [Daddy](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Daddy.php) | Done | [phergie/phergie-irc-plugin-react-daddy](https://github.com/dstockto/phergie-irc-plugin-react-daddy) |
| [Db](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Db.php) | **Open** | Could stand to be supplemented/replaced by a DBAL |
| [Dice](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Dice.php) | Done | [chrismou/phergie-irc-plugin-react-dice](https://github.com/chrismou/phergie-irc-plugin-react-dice) |
| [Encoding](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Encoding.php) | **Open** | Should be replaced with a library, third-party if available |
| [FeedManager](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/FeedManager.php) | **Open** | |
| [FeedParser](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/FeedParser.php) | Deprecated | Use [existing third-party feed parsing libraries](https://packagist.org/search/?q=feed) instead |
| [FeedTicker](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/FeedTicker.php) | Done | [phergie/phergie-irc-plugin-react-feedticker](https://github.com/phergie/phergie-irc-plugin-react-feedticker) |
| [Google](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Google.php) | Done | [chrismou/phergie-irc-plugin-react-google](https://github.com/chrismou/phergie-irc-plugin-react-google) |
| [Help](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Help.php) | Done | [phergie/phergie-irc-plugin-react-commandhelp](https://github.com/phergie/phergie-irc-plugin-react-commandhelp/) |
| <a name="http" />[Http](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Http.php) | Done | [wyrihaximus/phergie-http](https://github.com/WyriHaximus/PhergieHttp/) |
| [Ideone](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Ideone.php) | Blocked | Needs async SOAP client, e.g. [clue/php-soap-react](https://github.com/clue/php-soap-react) |
| [Invisible](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Invisible.php) | **Open** | Low-hanging fruit |
| [Join](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Join.php) | Done | [phergie/phergie-irc-plugin-react-joinpart](https://github.com/phergie/phergie-irc-plugin-react-joinpart) |
| [Karma](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Karma.php) | **Open** | |
| [Lart](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Lart.php) | **Open** | |
| [Logging](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Logging.php) | Deprecated | Now [supported in core](https://github.com/phergie/phergie-irc-bot-react/wiki/Development#logging) |
| [Message](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Message.php) | Deprecated | [phergie/phergie-irc-plugin-react-command](https://github.com/phergie/phergie-irc-plugin-react-command/) |
| [NickServ](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/NickServ.php) | Done | [phergie/phergie-irc-plugin-react-nickserv](https://github.com/phergie/phergie-irc-plugin-react-nickserv/) |
| [Part](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Part.php) | Done | [phergie/phergie-irc-plugin-react-joinpart](https://github.com/phergie/phergie-irc-plugin-react-joinpart/) |
| [Php](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Php.php) | In Progress | [chrismou/phergie-irc-plugin-react-php](https://github.com/chrismou/phergie-irc-plugin-react-php) |
| [Ping](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Ping.php) | Done | [renegade334/phergie-irc-plugin-react-ping](https://github.com/Renegade334/phergie-irc-plugin-react-ping/) |
| [PingPong](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/PingPong.php) | In Progress | [chrismou/phergie-irc-plugin-react-pingpong](https://github.com/chrismou/phergie-irc-plugin-react-pingpong) |
| [Pong](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Pong.php) | Done | [phergie/phergie-irc-plugin-react-pong](https://github.com/phergie/phergie-irc-plugin-react-pong) |
| [Prioritize](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Prioritize.php) | Deprecated | Now [supported in core](https://github.com/phergie/phergie-irc-bot-react/blob/master/src/EventQueue.php) |
| [Puppet](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Puppet.php) | Done | [pschwisow/phergie-irc-plugin-react-puppet](https://github.com/PSchwisow/phergie-irc-plugin-react-puppet)|
| [Quit](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Quit.php) | Done | [phergie/phergie-irc-plugin-react-quit](https://github.com/phergie/phergie-irc-plugin-react-quit) |
| [Reload](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Reload.php) | Deprecated | |
| [Remind](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Remind.php) | Done | [enebe-nb/phergie-irc-plugin-react-tell](https://github.com/enebe-nb/phergie-irc-plugin-react-tell) |
| [Serve](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Serve.php) | **Open** | |
| [SpellCheck](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/SpellCheck.php) | **Open** | |
| [Tea](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Tea.php) | Blocked | Needs Serve |
| [Temperature](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Temperature.php) | Deprecated | Use an [existing third-party measurement conversion library](https://packagist.org/search/?q=measure) instead |
| [TerryChay](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/TerryChay.php) | **Open** | Should use [Http](#http) |
| [TheFuckingWeather](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/TheFuckingWeather.php) | Done | [phergie/phergie-irc-plugin-react-thefuckingweather](https://github.com/phergie/phergie-irc-plugin-react-thefuckingweather) |
| [Time](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Time.php) | Deprecated | Use an [existing third-party time formatting library](https://packagist.org/search/?q=time) instead |
| [Tld](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Tld.php) | **Open** | |
| [Twitter](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Twitter.php) | In Progress |  [phergie/phergie-irc-plugin-react-twitter](https://github.com/phergie/phergie-irc-plugin-react-twitter) |
| [Url](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Url.php) | In Progress | [wyrihaximus/phergie-url](https://github.com/WyriHaximus/PhergieUrl) |
| [UrlShorten](https://github.com/phergie/phergie/tree/master/Phergie/Plugin/Url/Shorten) | Done | [pschwisow/phergie-irc-plugin-react-url-shorten](https://github.com/PSchwisow/phergie-irc-plugin-react-url-shorten) |
| [UserInfo](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/UserInfo.php) | Done | [phergie/phergie-irc-plugin-react-usermode](https://github.com/phergie/phergie-irc-plugin-react-usermode) |
| [Vend](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Vend.php) | In Progress | Should use [Http](#http) |
| [Weather](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Weather.php) | Done | [chrismou/phergie-irc-plugin-react-weather](https://github.com/chrismou/phergie-irc-plugin-react-weather) |
| [Wine](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Wine.php) | Blocked | Needs Serve |
| [Wunderground](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Wunderground.php) | Done | [chrismou/phergie-irc-plugin-react-weather](https://github.com/chrismou/phergie-irc-plugin-react-weather) |
| [YouTube](https://github.com/phergie/phergie/blob/master/Phergie/Plugin/Youtube.php) | Done | [phergie/phergie-irc-plugin-react-youtube](https://github.com/phergie/phergie-irc-plugin-react-youtube) |

## Ideas for New Plugins

Want a plugin that's not listed above? Suggest it by adding a row to the table below!

| Plugin  | Description | Suggested By | Claimed By |
| ------- | ----------- | ------------ | ---------- |
| BastardOperatorFromHell | Fetches a message from [this URL](http://pages.cs.wisc.edu/~ballard/bofh/bofhserver.pl) on command using the [Http](github.com/WyriHaximus/PhergieHttp) and [Command](github.com/phergie/phergie-irc-plugin-react-command) plugins | @elazar | -- |
| GitHub Issue Integration | OH in #slimphp: "what we need is hubot so I can type #17 and get that issue url from here" | @PSchwisow | -- |
| EventMonitor | Monitors what events are emitted, what channels they originate from, and ideally what plugins are invoked as a result, to get a sense of how the bot is being used | @elazar | -- |
| TableFlip | Output "(╯°□°）╯︵ ɹǝsodɯoɔ" where the phrase at the end is the upside-down version of a parameterized phrase | @elazar | @elstamey |
| UnitConvert | Uses a library like [this one](https://github.com/triplepoint/php-units-of-measure) to convert quantities between common units of measurement | @elazar | -- |
| Spotify Integration | Listen for Spotify URLs and show a (shortened) link and song info | @PSchwisow | -- |

Other ideas:

* Port plugins from [chunkybot](https://github.com/mculp/chunkybot)
