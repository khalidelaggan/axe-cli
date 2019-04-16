# axe-cli

[![Greenkeeper badge](https://badges.greenkeeper.io/dequelabs/axe-cli.svg)](https://greenkeeper.io/)

[![Join the chat at https://gitter.im/dequelabs/axe-core](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/dequelabs/axe-core?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Version](https://img.shields.io/npm/v/axe-cli.svg)](https://www.npmjs.com/package/axe-cli)
[![License](https://img.shields.io/npm/l/axe-cli.svg)](LICENSE)

Provides a command line interface for [aXe](https://github.com/dequelabs/axe-core) to run quick accessibility tests.

## Getting Started

Install [Node.js](https://docs.npmjs.com/getting-started/installing-node) if you haven't already. This project requires Node 6+. By default, axe-cli runs Chrome in headless mode, which requires Chrome 59 or up.

Install axe-cli globally: `npm install axe-cli -g`

Lastly, install the webdrivers of the browsers you wish to use. A webdriver is a driver for your web browsers. It allows other programs on your machine to open a browser and operate it. Current information about available webdrivers can be found at [selenium-webdriver project](https://www.npmjs.com/package/selenium-webdriver). Alternatively, you could use [Webdriver manager](https://www.npmjs.com/package/webdriver-manager)

## Usage

After installing, you can now run the `axe` command in your CLI, followed by the URL of the page you wish to test:

```
axe https://www.digital.canada.ca
```

You can run multiple pages at once, simply add more URLs to the command. Keep in mind that axe-cli is not a crawler, so if you find yourself testing dozens of pages at once, you may want to consider switching over to something like [axe-webdriverjs](https://www.npmjs.com/package/axe-webdriverjs). If you do not specify the protocol, http will be used by default:

```
axe https://www.digital.canada.ca, www.canada.ca
```

**Note:** If you are having difficulty with the color scheme, use `--no-color` to disable text styles.

## How it works 

It's a platform agnostic, it runs against anysite and any collections of URLs, so no code integrations required for any project.

It's using the Axe core JavaScript library and phantom JS to head this browser to go hit www.digital.canada.com, run Axe's accessibility rules, and then return a result.

Axe CLI is very helpfully reminding us that that doesn't cover all of it. You still need to test for accessibility manually. You need to test with the keyboard, make sure you can operate everything. You need to check the quality of your alt text and transcript content and things that require a human to review.

Depending on the size of the web page, it might take a second to complete, but this is a great tool that we could put in our continuous integration environment or just in our development workflow.

Something else we could do is tell it to use a different browser. Say, we wanted to run it with Selenium WebDriver and a real browser instance which would be more like a real user's experience. We could tell Axe CLI to use Chrome, Firefox, Edge, or whatever browser drivers you have installed on your machine.


## Running specific rules

You can use the `--rules` flag to set which rules you wish to run, or you can use `--tags` to tell axe to run all rules that have that specific tag. For example:

```
axe https://www.digital.canada.ca --rules color-contrast,html-has-lang
```

Or, to run all wcag2a rules:

```
axe https://www.digital.canada.ca --tags wcag2a
```

In case you want to disable some rules, you can use `--disable` followed by a list of rules. These will be skipped when analyzing the site:

```
axe https://www.digital.canada.ca --disable color-contrast
```

This option can be combined with either `--tags` or `--rules`.

A list of rules and what tags they have is available at: https://dequeuniversity.com/rules/worldspace/3.0/.

## Saving the results

Results can be saved as JSON data, using the `--save` and `--dir` flags. By passing a filename to `--save` you indicate how the file should be called. If no filename is passed, a default will be used. For example:

```
axe https://www.digital.canada.ca --save cds-site.json
```

Or:

```
axe https://www.digital.canada.ca --dir ./axe-results/
```

## Different browsers

Axe-cli can run in a variety of web browsers. By default axe-cli uses Chrome in headless mode. But axe-cli is equally capable of testing pages using other web browsers. **Running in another browser requires that browser's webdriver to be available on your PATH**. You can find a list of available webdrivers and how to install them at: https://seleniumhq.github.io/docs/wd.html

To run axe-cli using another browser, pass it in as the `--browser` option:

```
axe https://www.digital.canada.ca --browser chrome
```

Or for short:

```
axe https://www.digital.canada.ca -b c
```

## CI integration

Axe-cli can be ran within the CI tooling for your project. Many tools are automatically configured to halt/fail builds when a process exits with a code of `1`.

Use the `--exit` flag, `-q` for short, to have the axe-cli process exit with a failure code `1` when any rule fails to pass.

```
axe https://www.digital.canada.ca --exit
```

## Timing and timeout

For debugging and managing timeouts, there are two options available. With `--timer` set, axe-cli will log how long it takes to load the page, and how long it takes to run axe-core. If you find the execution of axe-core takes too long, which can happen on very large pages, use `--timeout` to increase the time axe has to test that page:

```
axe www.cnn.com --timeout=120
```

## Delay audit to ensure page is loaded

If you find your page is not ready after axe has determined it has loaded, you can use `--load-delay` followed by a number in milliseconds. This will make axe wait that time before running the audit after the page has loaded.

```
axe www.deque.com --load-delay=2000
```

## Reference 

This documntation is customized for CDS internal use to make it easy to understand this simple 101 applicable reporting tool that generate JSON file which contains the audit results for all accessibility violations for any given url.

Please refer to the axe-cli repo for full documentation https://github.com/dequelabs/axe-cli


