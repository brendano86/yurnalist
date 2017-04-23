# Yurnalist
The elegant console reporter, borrowed from [Yarn](https://yarnpkg.com).

## Introduction
Pretty console output makes people happy, and Yarn is doing a really nice job. Yurnalist isolates the part of Yarn responsible for handling the console and makes it available for standalone use in other Node.js commandline tools.

Yurnalist can be used to report many different things in the console besides simple messages, including progress bars, activity spinners, process steps, objects, lists, emojis, trees and tables.

User input for questions and selection is also available.

## Install
```shell
$ yarn add yurnalist
```
Or if your prefer NPM
```shell
$ npm install yurnalist
```

## How to use

```javascript
import report from 'yurnalist'

/* A function to fake some async task */
function doSomeWork(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function fetchSomething() {
  report.info('Wait while I fetch something for you');
  report.warn('It might take a little while though');

  const spinner = report.activity();
  spinner.tick('I am on it');

  try {
    await doSomeWork(1000);
    spinner.tick('Still busy');
    await doSomeWork(1000);
    spinner.tick('Almost there');
    await doSomeWork(1000);
    report.success('Done!');
  } catch (err) {
    report.error(err);
  }

  spinner.end();
}

fetchSomething();

```

## API
Coming soon...

### table
### step
### inspect
### list
### header
### footer
### log
### success
### error
### info
### command
### warn
### question
### tree
### activitySet
### activity
### select
### progress
### close
### createReporter


## Configuration
A normal import gives you a reporter instance configured with defaults for easy use. If you want something else you can call `createReporter(options)` to give you an instance with different options.

### Options

These are the options of the reporter as defined by Flow:

```javascript
type ReporterOptions = {
  verbose?: boolean,
  stdout?: Stdout,
  stderr?: Stdout,
  stdin?: Stdin,
  emoji?: boolean,
  noProgress?: boolean,
  silent?: boolean,
  peekMemoryCounter?: boolean,
};
```

The defaults used are:

```javascript
const defaults = {
  verbose: false,
  stdout: process.stdout,
  stderr: process.stderr,
  stdin: process.stdinn,
  emoji: true,
  noProgress: false,
  silent: false,
  peekMemoryCounter: false,
}
```

The peekMemoryCounter is disabled by default. If you enable it, you'll have to call `reporter.close()` to stop its running timer. Otherwise your program will not exit. The memory counter can be used to display in the footer data.

## Language
Yarn uses a language file for certain messages. For example if you try to skip a required question, or when you pick an invalid item from a select. This language file is not yet exposed in the Yurnalist API. The only supported language is English, as it is in Yarn at the moment.

I plan to make this configurable so that you can define your own messages in your own language .

## Examples
Examples showing different API functions are found in [/examples](/examples). You can run them directly with node >= 7.6 (because of async/await syntax). For older versions you could use the `--harmony` flag, or otherwise Babel.

To run the activity example:

```shell
$ node examples/activity.js
```

## Emojis
You can use Emojis in your output. Yurnalist should disable them if they are not allowed in the application environment.

Check:

- [node-emoji](https://github.com/omnidan/node-emoji)
- [Emoji cheat sheet](https://www.webpagefx.com/tools/emoji-cheat-sheet/)

## To Do
Before the 1.0 release there is still some work to be done. You can find an overview in the [1.0 milestone](https://github.com/0x80/yurnalist/milestone/1).

If you'd like to help out it is highly appreciated!

## Credits
Of course ❤️ and credits to all the contributers of [Yarn](https://yarnpkg.com). The ease with which I was able to extract this module from their codebase is proving some awesome engineering skills.

