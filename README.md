# Twig Plugin for Kirby CMS

A simple plugin to use the [Twig](http://twig.sensiolabs.org/) template engine with [Kirby](http://getkirby.com/).

:warning: This plugin no longer supports Kirby 1. To use it, specify `1.1.2` as the version when requiring.

:warning: This plugin requires at least Kirby 2.0.5.

## Installation

Use [Composer](https://getcomposer.org/) to install this plugin into the site plugins directory.

To use this plugin in a project, define the repository and require it.

```json
{
	"repositories": [
		{
			"type": "vcs",
			"url": "https://github.com/matthewspencer/kirby-twig-plugin"
		}
	],
	"require": {
		"matthewspencer/kirby-twig-plugin": "2.0.5",
		"composer/installers": "~1.0"
	},
	"extra": {
		"installer-paths": {
			"site/plugins/{$name}/": ["type:kirby-plugin"]
		}
	},
	"minimum-stability": "dev"
}
```

## Configuration

Set roots for vendor and twig, and the file extension for twig views in config.php.

```php
kirby()->roots->vendor = dirname( kirby()->roots()->site() ) . '/vendor';
kirby()->roots->twig = kirby()->roots()->site() . '/twig';
c::set( 'twig.file.extension', 'twig' );
```

## Usage

The template files in the Kirby templates directory handle passing the data to the appropriate Twig template. The following is an example of the default.php template.

```php
// use twig template name based on php template name
$template = $page->template() . '.' . c::get('twig.file.extension');

// pass an array of data to the twig template
echo $twig->render(
	$template,
	array(
		'site' => $site,
		'page' => array(
			'title' => $page->title,
			'template' => $page->template(),
			'text' => kirbytext($page->text()),
		),
	)
);
```

The above PHP passes data to a default.twig in the twig directory that was set in the config.

```twig
{% extends "base.twig" %}

{% block content %}

<section class="content">
	<article>
		<h1>{{ page.title }}</h1>
		{{ page.text }}
	</article>
</section>

{% endblock %}
```