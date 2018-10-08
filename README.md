# Shortcuts.js

[![GitHub release](https://img.shields.io/github/release/haykam821/Shortcuts.js.svg?style=popout&label=github)](https://github.com/haykam821/Shortcuts.js/releases/latest)
[![npm](https://img.shields.io/npm/v/shortcuts.js.svg?style=popout&colorB=red)](https://www.npmjs.com/package/shortcuts.js)
[![Travis (.com)](https://img.shields.io/travis/com/haykam821/Shortcuts.js.svg?style=popout)](https://travis-ci.com/haykam821/Shortcuts.js)

A simple library for Apple's Shortcuts. 

## Installation

You can install this module with NPM:

    npm install shortcuts.js

If you prefer [Yarn](https://yarnpkg.com/), you can use that as well:

    yarn add shortcuts.js

## Usage

First, you must require this library:

```js
const shortcuts = require("shortcuts.js");
```

Afterwards, you can use its methods. One of the simplest things you can do with this library is grab the ID from an iCloud URL. This is useful for later methods such as getting a shortcut's details from a user-submitted value, which might not always be just the ID.

```js
// Get an ID from a iCloud URL
const id = shortcuts.idFromURL("https://www.icloud.com/shortcuts/903110dea9a944f48fef9e94317fb686");
```

This example uses promise chaining to get shortcut metadata from an iCloud URL:

```
// Chain promises to get a shortcut's metadata
shortcuts.getShortcutDetails(id).then(shortcut => {
    console.log(`Hi, I'm ${id}! What's your name?`);
    return shortcut.getMetadata();
}).then(metadata => {
    console.log(`I have ${metadata.actions.length} actions! How cool is that?`);
}).catch(error => {
    console.log(`${error.code}? How could this happen!`);
});
```

You can also use [async/await](https://javascript.info/async-await) to simplify things:

```
async function getBasicInfo() {
    const shortcut = await shortcuts.getShortcutDetails(id);
    const metadata = await shortcut.getMetadata();
    
    return `Hi! I'm ${shortcut.name}. I have ${metadata.importQuestions.length} import questions, and I'm happy to be here. What's your name?`;
}

getBasicInfo().then(console.log).catch(error => {
    console.log(`There was an error: ${error.code}. At least I can say that I tried...`);
});
```

## Documentation

This is the documentation for Shortcuts.js.

## Quick Reference

Click any link below to quickly jump to its documentation:

* [Library Methods](#main-api)
* [Shortcut Properties](#shortcut-properties-and-methods)
* [ShortcutMetadata Properties](#shortcutmetadata-properties)
* [Action Properties](#action-properties)
* [ImportQuestion Properties](#importquestion-properties)

## Main API

These are methods exposed on the libary. Once you include the libary you can call `shortcuts.<Method>(Param)` and expect the appropriate return value.

### [`getShortcutDetails(id)`](https://github.com/haykam821/Shortcuts.js/blob/743f032ccd63daa83c0ffd439002dd99a47c9a1f/index.js#L235)

This method gets the details of a shortcut.

The `id` argument is required and specifies the public ID of the shortcut to fetch. It can be found in the shareable URLs, in the form of `https://icloud.com/shortcuts/<id>`.

This method immediately returns a promise, which will resolve to a `Shortcut`.

### [`idFromURL(url, allowSingle)`](https://github.com/haykam821/Shortcuts.js/blob/743f032ccd63daa83c0ffd439002dd99a47c9a1f/index.js#L277)

Gets a shortcut ID from its URL.

This method returns either `false` or a string. If it returns a string, that string will be the parsed ID from the URL, or `false` if the URL was unable to be parsed.

The `url` is a string that can take on multiple forms:

  * Just the ID (`903110dea9a944f48fef9e94317fb686`)
  * An iCloud link (`http://icloud.com/shortcuts/903110dea9a944f48fef9e94317fb686`)
  * An iCloud link with the `https` protocol (`https://icloud.com/shortcuts/903110dea9a944f48fef9e94317fb686`)
  * An iCloud link with the `www` subdomain (`http://www.icloud.com/shortcuts/903110dea9a944f48fef9e94317fb686`)

### [`Shortcut`](https://github.com/haykam821/Shortcuts.js/blob/743f032ccd63daa83c0ffd439002dd99a47c9a1f/index.js#L144) properties and methods

This is what the `getShortcutDetails()` promise will resolve with.

`name` ⟶ String

The user-specified name of the shortcut.

`longDescription` ⟶ String

The long description of the shortcut.
This does not seem to be settable by users.

`creationDate` ⟶ Date

The date that the shortcut was created.

`modificationDate` ⟶ Date

The date that the shortcut was last modified on.

`downloadURL` ⟶ String

The URL to download the shortcut as a PLIST

`icon` ⟶ Object

Details of the icon of this shortcut.

`response` ⟶ Object

The full API response.

`id` ⟶ String

The ID of the shortcut.

`getLink()` ⟶ String

Gets the shortcut's landing page URL.

`getMetadata()` ⟶ Promise\<ShortcutMetadata>

Downloads and parses the shortcut's metadata.

### [`ShortcutMetadata`](https://github.com/haykam821/Shortcuts.js/blob/743f032ccd63daa83c0ffd439002dd99a47c9a1f/index.js#L79) properties

The shortcut file with all the properties parsed into a custom class. The object will be found after calling `getMetadata()` on a shortcut.

`client` ⟶ Object

Details of the client used to create this shortcut.

`icon` ⟶ Object

Details of the icon of this shortcut.

`importQuestions` ⟶ ImportQuestion[]

The user-specified name of the shortcut.

`types` ⟶ String[]

An unknown property.

`actions` ⟶ Action[]

A list of actions that the shortcut performs.

`inputContentItemClasses` ⟶ String[]

A list of services that the shortcut uses.

### [`Action`](https://github.com/haykam821/Shortcuts.js/blob/743f032ccd63daa83c0ffd439002dd99a47c9a1f/index.js#L22) properties

A single action in a shortcut. The object will be found in the `ShortcutMetadata.actions` property.

`identifier` ⟶ String

A namespace of the action.

`parameters` ⟶ Object

An object denoting a shortcut action. The properties in this object are not renamed and follow the Apple naming convention.


### [`ImportQuestion`](https://github.com/haykam821/Shortcuts.js/blob/743f032ccd63daa83c0ffd439002dd99a47c9a1f/index.js#L42) properties

A single question to be asked when importing a shortcut into the Shortcuts app. The object will be found in the `ShortcutMetadata.importQuestions` property.

`parameterKey` ⟶ String

An unknown property.

`category` ⟶ String

The category of the question.

`actionIndex` ⟶ Number

The action index of the question.

`text` ⟶ String

The question to be asked.

`defaultValue` ⟶ String

The default value for the question to be asked.
