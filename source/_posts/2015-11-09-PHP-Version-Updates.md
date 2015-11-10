---
title: "PHP Version Updates and Development"
tags:
    - news
categories:
    - news
---
Hello everyone! Some updates from the Phergie team:

### Minimum PHP Versions

We are in the process of removing all support for any PHP version older than 5.5. We will no longer be testing any older versions. We hope this helps push users forward to newer versions of PHP. 

With the imminent release of PHP 7 we have removed the allowed failures for 7 on Travis.


### Development Package!

We have created a development package [phergie/phergie-irc-bot-react-development](https://github.com/phergie/phergie-irc-bot-react-development) to hold all of our commonly used development dependencies and have begun updating packages to use this.

Instead of requiring multiple dependencies for development you can require this one package instead.