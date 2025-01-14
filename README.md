# Kirby CLI

The Kirby command line interface helps simplifying common tasks with your Kirby installations.

![kirby-cli](https://user-images.githubusercontent.com/24532/193451850-73b1404d-82e2-42d4-b62b-3122fdc3f005.png)


## Installation

### Via Composer

```
composer global require getkirby/cli
```

Make sure to add your composer bin directory to your `~/.bash_profile` (Mac OS users) or into your `~/.bashrc` (Linux users).

Your global composer directory is normally either `~/.composer/vendor/bin` or `~/.config/composer/vendor/bin`. You can find the correct path by running …

```
composer -n config --global home
```

Afterwards, add the result to your bash profile  …

```
export PATH=~/.composer/vendor/bin:$PATH
```

### Did it work?

Check if the installation worked by running the following in your terminal.

```
kirby
```

This should print the Kirby CLI version and a list of available commands

## Available core commands

```
- kirby clear:cache
- kirby clear:media
- kirby clear:sessions
- kirby download
- kirby help
- kirby install
- kirby install:kit
- kirby install:repo
- kirby make:blueprint
- kirby make:collection
- kirby make:command
- kirby make:config
- kirby make:controller
- kirby make:model
- kirby make:plugin
- kirby make:snippet
- kirby make:template
- kirby remove:command
- kirby unzip
- kirby uuid:generate
- kirby uuid:populate
- kirby version
```

## Writing commands

You can create a new command via the CLI:

```bash
kirby make:command hello
```

This will create a new `site/commands` folder in your installation with a new `hello.php` file

The CLI will already put the basic scaffolding into the file:

```php
<?php

return [
    'description' => 'Nice command',
    'args' => [],
    'command' => static function ($cli): void {
        $cli->success('Nice command!');
    }
];
```

You can define your command logic in the command callback. The `$cli` object comes with a set of handy tools to create output, parse command arguments, create prompts and more.

## Global commands

You might have some commands that you need for all your local Kirby installations. This is where global commands come in handy. You can create a new global command with the `--global` flag:

```bash
kirby make:command hello --global
```

The command file will then be place in `~/.kirby/commands/hello.php` and is automatically available everywhere.

## Check for installed commands

You can always check back if your commands have been created properly by running `kirby` again

```
kirby
```

## Removing commands

Once you no longer need a command, you can remove it with …

```bash
kirby remove:command hello
```

If you have a local and a global command, you can choose which one to delete.

## Formatting Output

Sending messages to the terminal is super easy.

### $cli->out()

```php
$cli->out('This is some simple text');
```

### $cli->success()

```php
$cli->success('This is text in a nice green box');
```

### $cli->error()

```php
$cli->error('This is red text for errors');
```

### $cli->bold()

```php
$cli->bold('This is some bold text');
```

### $cli->br()

```php
// this will create a line break
$cli->br();
```

For more available colors and formats, check out the CLImate docs: https://climate.thephpleague.com/styling/colors/

## Arguments

Your commands can define a list of required and optional arguments that need to be provided by the user.

```php
<?php

return [
    'description' => 'Hello world',
    'args' => [
        'name' => [
            'description' => 'The name for the greeting',
            'required'    => true
        ]
    ],
    'command' => static function ($cli): void {
        $cli->success('Hello ' . $cli->arg('name') . '!');
    }
];
```

The command can now be executed by providing the name …

```
kirby hello Joe
```

If no name is provided, an error will be shown.

### Argument docs

Arguments can be required, can set a default value and more. Check out the CLImate docs for additional options: https://climate.thephpleague.com/arguments/

## Prompts

Instead of taking arguments from the command, you can also ask for them in a prompt:

```php
<?php

return [
    'description' => 'Hello world',
    'command' => static function ($cli): void {
        $name = $cli->prompt('Please enter a name:');
        $cli->success('Hello ' . $name . '!');
    }
];
```

As a third alternative you can either take the argument or ask for it if it is not provided:

```php
<?php

return [
    'description' => 'Hello world',
    'args' => [
        'name' => [
            'description' => 'The name for the greeting',
        ]
    ],
    'command' => static function ($cli): void {
        $name = $cli->argOrPrompt('name', 'Please enter a name:');
        $cli->success('Hello ' . $name . '!');
    }
];
```

## Checkboxes Radios and more

The CLI also supports more complex ways to get input from users. Check out the CLImate docs how to work with user input: https://climate.thephpleague.com/terminal-objects/input/

## Combining commands

You can reuse all existing commands in your custom commands to create entire chains of actions.

```php
<?php

return [
    'description' => 'Downloads the starterkit and the plainkit',
    'command' => static function ($cli): void {

        $cli->command('install:kit', 'starterkit');
        $cli->command('install:kit', 'plainkit');

        $cli->success('Starterkit and plainkit have been installed');
    }
];
```

## kirby.cli.json

You can place a kirby.cli.json file in root folder of your project to setup the Kirby instance with custom directories. This is useful if you use a non-standard Kirby installation with a public webroot for example.

```json
{
    "roots": {
        "index": "./public",
    }
}
```

### Where to place local commands

The config can also set where local commands are stored. By default, they are stored in `/site/commands`

```json
{
    "roots": {
        "commands.local": "./commands",
        "commands.global": "/var/kirby/commands",
    }
}
```

## What's Kirby?

- **[getkirby.com](https://getkirby.com)** – Get to know the CMS.
- **[Try it](https://getkirby.com/try)** – Take a test ride with our online demo. Or download one of our kits to get started.
- **[Documentation](https://getkirby.com/docs/guide)** – Read the official guide, reference and cookbook recipes.
- **[Issues](https://github.com/getkirby/kirby/issues)** – Report bugs and other problems.
- **[Feedback](https://feedback.getkirby.com)** – You have an idea for Kirby? Share it.
- **[Forum](https://forum.getkirby.com)** – Whenever you get stuck, don't hesitate to reach out for questions and support.
- **[Discord](https://chat.getkirby.com)** – Hang out and meet the community.
- **[YouTube](https://youtube.com/kirbyCasts)** - Watch the latest video tutorials visually with Bastian.
- **[Twitter](https://twitter.com/getkirby)** – Spread the word.
- **[Instagram](https://www.instagram.com/getkirby/)** – Share your creations: #madewithkirby.

---

© 2009-2022 Bastian Allgeier
[getkirby.com](https://getkirby.com) · [License agreement](./LICENSE.md)





