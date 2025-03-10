# PagerDuty Event notifications channel for Laravel

[![Latest Version on Packagist](https://img.shields.io/packagist/v/laravel-notification-channels/pagerduty.svg?style=flat-square)](https://packagist.org/packages/laravel-notification-channels/pagerduty)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Build Status](https://img.shields.io/travis/laravel-notification-channels/pagerduty/master.svg?style=flat-square)](https://travis-ci.org/laravel-notification-channels/pagerduty)
[![StyleCI](https://styleci.io/repos/90993408/shield)](https://styleci.io/repos/90993408)
[![SensioLabsInsight](https://img.shields.io/sensiolabs/i/320fd214-7e74-4f71-ab10-f3f979e01a10.svg?style=flat-square)](https://insight.sensiolabs.com/projects/320fd214-7e74-4f71-ab10-f3f979e01a10)
[![Quality Score](https://img.shields.io/scrutinizer/g/laravel-notification-channels/pagerduty.svg?style=flat-square)](https://scrutinizer-ci.com/g/laravel-notification-channels/pagerduty)
[![Code Coverage](https://img.shields.io/scrutinizer/coverage/g/laravel-notification-channels/pagerduty/master.svg?style=flat-square)](https://scrutinizer-ci.com/g/laravel-notification-channels/pagerduty/?branch=master)
[![Total Downloads](https://img.shields.io/packagist/dt/laravel-notification-channels/pagerduty.svg?style=flat-square)](https://packagist.org/packages/laravel-notification-channels/pagerduty)

This package makes it easy to send notification events to [PagerDuty](https://www.pagerduty.com) with Laravel 11.x+

## Contents

- [Installation](#installation)
	- [Setting up the PagerDuty service](#setting-up-the-PagerDuty-service)
- [Usage](#usage)
    - [PagerDuty Setup](#pagerduty-setup)
	- [Available Message methods](#available-message-methods)
- [Changelog](#changelog)
- [Testing](#testing)
- [Security](#security)
- [Contributing](#contributing)
- [Credits](#credits)
- [License](#license)


## Installation

You can install the package via composer:

```bash
composer require laravel-notification-channels/pagerduty
```

## Usage

Now you can use the channel in your `via()` method inside the Notification class.

```php
use NotificationChannels\PagerDuty\PagerDutyChannel;
use NotificationChannels\PagerDuty\PagerDutyMessage;
use Illuminate\Notifications\Notification;

class SiteProblem extends Notification
{
    public function via($notifiable)
    {
        return [PagerDutyChannel::class];
    }

    public function toPagerDuty($notifiable)
    {
        return PagerDutyMessage::create()
            ->setSummary('There was an error with your site in the {$notifiable->service} component.');
    }
}
```

In order to let your Notification know which Integration should receive the event, add the `routeNotificationForPagerDuty` method to your Notifiable model.

This method needs to return the Integration Key for the service and integration to which you want to send the event.

```php
public function routeNotificationForPagerDuty()
{
    return '99dc10c97a6e43c387bbc4f877c794ef';
}
```

### PagerDuty Setup
On a PagerDuty Service of your choice, create a new Integration using the `Events API v2`.

![Creating a new integration](doc/CreateNewIntegration.png)

The `Integration Key` listed for your new integration is what you need to set in the `routeNotificationForPagerDuty()` method.

![List of Integrations with Keys](doc/ListIntegrations.png)

### Available Message methods

- `resolve()`: Sets the event type to `resolve` to resolve issues.
- `setDedupKey('')`: Sets the `dedup_key` (required when resolving).
- `setSummary('')`: Sets a summary message on the event.
- `setSource('')`: Sets the event source; defaults to the `hostname`.
- `setSeverity('')`: Sets the event severity; defaults to `critical`.
- `setTimestamp('')`: Sets the `timestamp` of the event.
- `setComponent('')`: Sets the `component` of the event.
- `setGroup('')`: Sets the `group` of the event.
- `setClass('')`: Sets the `class`.
- `addCustomDetail('', '')`: Adds a key/value pair to the `custom_detail` of the event.

See the [PagerDuty v2 Events API documentation](https://v2.developer.pagerduty.com/docs/send-an-event-events-api-v2)
for more information about what these options will do.

## Usage

```php
Notification::route('PagerDuty', '[my integration key]')->notify(new BasicNotification);
```

_When using `Notification::route` be sure to reference 'PagerDuty' as the Channel._

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Testing

``` bash
$ composer test
```

## Security

If you discover any security related issues, please email lwaite@gmail.com instead of using the issue tracker.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Credits

- [Luke Waite](https://github.com/lukewaite)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
