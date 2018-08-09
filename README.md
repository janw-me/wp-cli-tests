wp-cli/wp-cli-tests
===================

WP-CLI testing framework

[![Build Status](https://travis-ci.org/wp-cli/wp-cli-tests.svg?branch=master)](https://travis-ci.org/wp-cli/wp-cli-tests)

Quick links: [Using](#using) | [Contributing](#contributing) | [Support](#support)

## Using

To make use of the WP-CLI testing framework, you need to complete the following steps from within the package you want to add them to:

1. Add the testing framework as a development requirement:
	```bash
	composer require --dev wp-cli/wp-cli-tests
	```

2. Add the required test scripts to the `composer.json` file:
	```json
	"scripts": {
        "behat": "run-behat-tests",
        "behat-rerun": "rerun-behat-tests",
        "lint": "run-linter-tests",
        "phpcs": "run-phpcs-tests",
        "phpunit": "run-php-unit-tests",
        "prepare-tests": "install-package-tests",
        "test": [
            "@lint",
            "@phpcs",
            "@phpunit",
            "@behat"
        ]
	}
	```
	You can of course remove the ones you don't need.

3. Optionally add a modified process timeout to the `composer.json` file to make sure scripts can run until their work is completed:
	```json
	"config": {
		"process-timeout": 1800
	},
	```
	The timeout is expressed in seconds.

4. Optionally add a `behat.yml` file to the package root with the following content:
	```json
	default:
	  paths:
	    features: features
        bootstrap: vendor/wp-cli/wp-cli-tests/features/bootstrap
	```
	This will make sure that the automated Behat system works across all platforms. This is needed on Windows.

5. Update your composer dependencies and regenerate your autoloader and binary folders:
	```bash
	composer update
	```

You are now ready to use the testing framework from within your package.

### Launching the tests

You can use the following commands to control the tests:

* `composer prepare-tests` - Set up the database that is needed for running the functional tests. This is only needed once.
* `composer test` - Run all test suites.
* `composer lint` - Run only the linting test suite.
* `composer phpcs` - Run only the code sniffer test suite.
* `composer phpunit` - Run only the unit test suite.
* `composer behat` - Run only the functional test suite.

### Controlling what to test

To send one or more arguments to one of the test tools, prepend the argument(s) with a double dash. As an example, here's how to run the functional tests for a specific feature file only:
```bash
composer behat -- features/cli-info.feature
```

Prepending with the double dash is needed because the arguments would otherwise be sent to Composer itself, not the tool that Composer executes.

### Controlling the test environment

#### WordPress Version

You can run the tests against a specific version of WordPress by setting the `WP_VERSION` environment variable.

This variable understands any numeric version, as well as the special terms `latest` and `trunk`.

Note: This only applies to the Behat functional tests. All other tests never load WordPress.

Here's how to run your tests against the latest trunk version of WordPress:
```bash
WP_VERSION=trunk composer behat
```

#### WP-CLI Binary

You can run the tests against a specific WP-CLI binary, instead of using the one that has been built in your project's `vendor/bin` folder.

This can be useful to run your tests against a specific Phar version of WP_CLI.

To do this, you can set the `WP_CLI_BIN_DIR` environment variable to point to a folder that contains an executable `wp` binary. Note: the binary has to be named `wp` to be properly recognized.

As an example, here's how to run your tests against a specific Phar version you've downloaded.
```bash
# Prepare the binary you've downloaded into the ~/wp-cli folder first.
mv ~/wp-cli/wp-cli-1.2.0.phar ~/wp-cli/wp
chmod +x ~/wp-cli/wp

WP_CLI_BIN_DIR=~/wp-cli composer behat
```

### Setting up the tests in Travis CI



#### WP-CLI version

You can point the tests to a specific version ow WP-CLI through the `WP_CLI_BIN_DIR` constant:
```bash
WP_CLI_BIN_DIR=~/my-custom-wp-cli/bin composer behat
```

#### WordPress version

If you want to run the feature tests against a specific WordPress version, you can use the `WP_VERSION` constant:
```bash
WP_VERSION=4.2 composer behat
```

The `WP_VERSION` constant also understands the `latest` and `trunk` as valid version targets.

## Contributing

We appreciate you taking the initiative to contribute to this project.

Contributing isn’t limited to just code. We encourage you to contribute in the way that best fits your abilities, by writing tutorials, giving a demo at your local meetup, helping other users with their support questions, or revising our documentation.

For a more thorough introduction, [check out WP-CLI's guide to contributing](https://make.wordpress.org/cli/handbook/contributing/). This package follows those policy and guidelines.

### Reporting a bug

Think you’ve found a bug? We’d love for you to help us get it fixed.

Before you create a new issue, you should [search existing issues](https://github.com/wp-cli/wp-cli-tests/issues?q=label%3Abug%20) to see if there’s an existing resolution to it, or if it’s already been fixed in a newer version.

Once you’ve done a bit of searching and discovered there isn’t an open or fixed issue for your bug, please [create a new issue](https://github.com/wp-cli/wp-cli-tests/issues/new). Include as much detail as you can, and clear steps to reproduce if possible. For more guidance, [review our bug report documentation](https://make.wordpress.org/cli/handbook/bug-reports/).

### Creating a pull request

Want to contribute a new feature? Please first [open a new issue](https://github.com/wp-cli/wp-cli-tests/issues/new) to discuss whether the feature is a good fit for the project.

Once you've decided to commit the time to seeing your pull request through, [please follow our guidelines for creating a pull request](https://make.wordpress.org/cli/handbook/pull-requests/) to make sure it's a pleasant experience. See "[Setting up](https://make.wordpress.org/cli/handbook/pull-requests/#setting-up)" for details specific to working on this package locally.

## Support

Github issues aren't for general support questions, but there are other venues you can try: https://wp-cli.org/#support


