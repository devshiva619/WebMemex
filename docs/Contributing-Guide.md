# <a id="contributing" href="#contributing">Contributing</a>

**If you are a Developer head to [Getting Started](#getting-started)**

**If you are a Designer, Marketer, or Investor**
    Please Email Oliver Sauter: oli@worldbrain.io

**If you would like to donate a bit of money**
Please support us on [Patreon](https://www.patreon.com/WorldBrain) :moneybag:

## Getting Started

We have broken up tasks into three levels, **easy**, **medium** and **hard** tasks. You can choose based on your skill level or how much time you would like to put into contributing. **After finding a Task you would like to help out with please see [Installation Instructions](#installation)**.


#### [Priorities :exclamation:](https://github.com/WorldBrain/WebMemex/issues?q=is%3Aissue+is%3Aopen+label%3A%22Prio+1%22)
#### [Bugs :space_invader:](https://github.com/WorldBrain/WebMemex/issues?utf8=%E2%9C%93&q=is%3Aissue%20is%3Aopen%20label%3Abug)
#### [Enhancements :muscle::point_up:](https://github.com/WorldBrain/WebMemex/issues?q=is%3Aissue+is%3Aopen+label%3Aenhancement)



## Installation

**This assumes a basic knowledge of `git`, `npm` and usage of the `command line`.**

### First steps:
**Clone this repo:**

```sh
$ git clone https://github.com/WorldBrain/WebMemex
```

**Install yarn:**
```sh
$ npm install -g yarn
```

**Run `yarn` to install dependencies**
```sh
$ yarn
```
**This could take a while....**

**Now run `yarn watch` to compile incremental builds**
```sh
$ yarn watch
```

**For Windows Users! Please run: **TODO****
```sh
$ yarn watch **TODO**
```

## Running The Extension

As of now it should work in most modern browsers except Safari (we mainly use Chrome for testing so if any inconsistencies are found in Firefox, Opera or any other browser please create an [issue](https://github.com/WorldBrain/WebMemex/issues/new) and submit a fix)

**Note: It is highly recommended to [create a new browser profile](#creating-a-new-browser-profile) for dev purposes especially if you currently use the extension and would like to develop without interfering with its daily use**

### Creating a New Browser Profile
*Chrome:*
1. In the top right corner of your browser click the user icon (above the menu). Alternatively go into [settings](chrome://settings/)
2. Now click **Manage People**, in settings: Under *People* click **Manage other people**
3. Now click **Add People** in the lower left
4. Choose a name such as **Worldbrain Test** and any icon.
5. All Finished! :tada:

*Firefox:*
1. In firefox url type in [about:profiles](about:profiles)
2. Click `Create a new Profile`
3. Hit `continue`
4. Now enter a name such as **Worldbrain Test** and hit `continue`
5. You should see the new Profile and can now click `Launch profile in new browser`
6. All Finished! :tada: For more info see: [creating multiple profiles](https://developer.mozilla.org/en-US/Firefox/Multiple_profiles)

### Running + Debugging
*Chrome*
1. Open a `New Tab`
3. Type [chrome://extensions](chrome://extensions) into the address bar
5. At this point, it is recommended to bookmark `Extensions` for ease of use in development
2. Check `developer mode` box
6. Click `Load unpacked extension...`
7. Now navigate to the folder where you cloned the repo and there should be a new folder named extension (this was created by [`yarn watch`]) go into this folder then click `select this folder`
8. Everything should be all loaded! 😃
9. To view developer tools go to the [Extension Page](chrome://extensions/) and under *inspect views:* click `background page`

**Note:** This is only for debugging the [background scripts](#code-overview). [Content-Script](#code-overview]) works within the reg dev tools of any given tab and the [Options](#srcoptions-the-settings-page), [Overview](#srcoverview-overview) and [Popup](#srcpopup-extension-popup) UI dev tools can be accesed by `right click Inspect` on the given element.

*Firefox*
1. Enter [about:debugging](about:debugging) into the address bar
2. Check the `Enable add-on debugging` box
3. Click `Load Temporary Add-on`
4. Now navigate to the folder where you cloned the repo and there should be a new folder named extension (this was created by [`yarn watch`]) go into this folder select the `manifest.json` file and then click `open`
5. Everything should be all loaded! 😃
6. To view the developer tools simply click `Debug` under the Worldbrain Extension in [about:debugging](about:debugging)


**Now you are ready to hack! 😃**
We recommend reading through the [Code Overview](#code-overview) to get an idea of how the extension works and also looking @ [Submitting Changes](#submitting-changes) before making any pull requests.

## Submitting Changes

1. Before making **any changes**
    - Write a Readme and/or Update Existing Readme. We use a [Readme First Development Approach](http://tom.preston-werner.com/2010/08/23/readme-driven-development.html).
    - Create a New Branch
    - Make a pull-request with your Initial Readme
    - Add a to-do list to help everyone be on board with the changes you plan on making.
    - Start Coding :computer::punch:
3. Please Always re-base after finishing up your branch before merging :recycle:
5. We encourage you to message us when you are ready to merge or at least indicate you are finished working on your branch, that way things happen quickly and efficently, otherwise pull requests can get old :older_woman::older_man:


### Documenting
If you have made changes to any code or modules please update the corresponding docs **Pull Requests will not be merged otherwise**.

If you are creating a new 'module' (folder in src i.e. src/new_folder/)
Please add some documentation, See [Doc-Template](./Readme-Template.md)

### Styling
We are using [prettier](https://github.com/prettier/prettier). With some hooks for eslint.
This will automatically format all the styling for the code every time a merge request is made. So you can focus on coding and not on styling.


# <a id="code-overview" href="#code-overview">Code Overview</a>

## A brief overview of Web Extensions

A web extension consists of three main parts:

- `background.js` always runs, in an 'empty invisible tab', listening for
  messages and events.
- `content_script.js` is loaded into every web page that is visited. It is
  invisible from that web page's own scripts, and can talk to the background script.

- **User Interfaces**, The UI's are set up and declared in the `extension/manifest.json` file and at the moment consist of four elements the Popup, Overview, Options and Omnibar.

The parts communicate in three ways:
- Messaging through `browser.runtime.sendMessage`, usually done implicitly by using a remote procedure call ([`util/webextensionRPC.js`](../src/util/webextensionRPC.js)).
- Bidirectional messaging using `browser.runtime.connect`. We use this to communicate between Overview UI script and background script, and also the deprecated imports UI (via Options UI script) and background script. See [Runtime] (https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API/runtime) for more info
- Through the in-browser PouchDB database, they get to see the same data and can react to changes made by other parts.

Besides these parts:
[`browser-polyfill.js`](https://github.com/mozilla/webextension-polyfill/)
provides support for the `WebExtension/BrowserExt API` in order to make the same code run in different browsers (and to structure the callback mess).

This API is available in Chrome/Chromium by default (under `window.chrome`) but is meant to be developed and standardized as it's own thing.

**For more info please see [Anatomy of a WebExtension](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Anatomy_of_a_WebExtension)**

## Application Structure
To keep things modular, the source code in [`src/`](../src/) is split into folders which are grouped by functionality. They can almost be thought of as separate libraries and some folders may end up being factored out into their own repos later on.

#### **[src/blacklist/](../src/blacklist/)**: blacklist

This allows a user to stop and/or delete the index/logging of specific domains or pages they visit.

#### **[src/bookmarks](../src/bookmarks)**: Bookmarks-related logic

Contains all the logic related to handling our data structure for bookmarks, along with background event listeners for handling browser bookmark creation.

#### **[src/common-ui/components](../src/common-ui/components)**: ui elements

This contains common user interface elements such as the loading indicator.

#### **[src/dev/](../src/dev/)**: development tools

Tools to help during development. They are not used in production builds.

#### **[src/imports/](../src/imports/)**: browser history import

This allows users to import their whole browser history, however, due to slow speeds and multiple setbacks it has been deprecated and may only be kept on as a dev tool to import test docs.

Instead of using a browser this module is set to move to a native Desktop Application see [roadmap](https://trello.com/b/mdqEuBjb) for more details

#### **[src/options](../src/options/)**: the settings page

This shows the settings page of the extension which includes (for the time being)
 - Blacklist
 - Acknowledgements
 - Privacy
 - Help Me Please

#### **[src/overview/](../src/overview/)**: overview

The `overview` is a user interface that opens in its own tab. It shows a 'facebook newsfeed' like page that lists random pages you have visited. It also provides tools for more advanced search options and a complete search overview with screenshots. It is built with [React](https://facebook.github.io/react/) and [Redux](http://redux.js.org/).

See [The Docs](../src/overview/Readme.md) for more details.

#### **[src/page-analysis](../src/page-analysis/)**: page analysis

This extracts and stores information about the page in a given tab, such as:
- The plain text of the page, mainly for the full-text
search index.
- Metadata, such as its author, publication date, etc...
- A screenshot for visual recognition.

See [The Docs](../src/page-analysis/Readme.md) for more details.

#### **[src/page-storage](../src/page-storage/)**: document search

This runs through each page to store and makes sure there isn't a duplicate and prepares it for use in our data model

See [The Docs](../src/page-storage/Readme.md) for more details.

#### **[src/popup](../src/popup/)**: extension popup

The `popup` is a mini UI that pops up when you click on the worldbrain. It contains
- search
- pause
- settings
- feedback

See [The Docs](../src/popup/Readme.md) for more details.

#### **[src/search](../src/search/)**: document search

This provides a **Full-Text-Search** through all the web pages a user visits.

See [The Docs](../src/search/Readme.md) for more details.

#### **[src/util/](../src/util)**: utilities

Contains small generic things, stuff that is not project-specific. Things that
could perhaps be packaged and published as an NPM module some day.
See [The Docs](../src/util/Readme.md) for more details.

#### **[src/background.js](../src/background.js)**: WebExtension background script

This provides a wrapper for all the process run in the background which sits in an empty tab listening for events.

See [A brief overview of web extensions](#AbriefoverviewofWebExtensions) for more details.

#### **[src/content_script.js](../src/content-script.js)**: A Part of Web Extensions

This just wraps the content scripts of activity-logger and page analysis for use as a [Web Extension](#AbriefoverviewofWebExtensions).

#### **[src/omnibar.js](../src/omnibar.js)**: Search-Bar Controls

Allows a user to type `w` + `space_bar` to search through worldbrain without leaving the search bar.

See (Mozilla Omnibox Docs)[https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API/omnibox] for more details

#### **[src/pouchdb.js](../src/omnibar.js)**: Our Persistent Browser Database

This initializes the user database using [PouchDB](https://pouchdb.com/)

#### **...**: other stuff

The build process is based on `yarn`, that runs some `npm` commands specified in
`package.json`, which in turn start the corresponding tasks in
`gulpfile.babel.js` (transpiled by settings in `.babelrc`).


## Dependencies

- [react](https://reactjs.org/) - A  Javascript component-based 'framework' (it's actually a library) used for the User Interface
- [react-redux](https://github.com/reactjs/react-redux) - A global state handler that syncs with react to create a nice workflow.
- [babel](https://babeljs.io/) - This is used for compiling ES7=>6=>5
- [browserify](http://browserify.org/) - Combined with babel allows us to bundle up our dependencies using the `import 'module'` syntax
- [gulp](https://gulpjs.com/) - Combined with browserify + babel this gives us the ability to listen to any saved changes in our code and automatically compile it for use in a Browser Extension
