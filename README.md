# markdown-formatter

> A markdown formatter intended for writing specifications

<!-- TOC START min:2 max: 4 -->

* [Badges and stuff](#badges-and-stuff)

  * [Info](#info)
  * [Status](#status)

* [What it is](#what-it-is)

* [Use it](#use-it)

  * [CLI](#cli)

    * [Options](#options)

  * [API](#api)

    * [formatFromString](#formatfromstring)
    * [formatFromFile](#formatfromfile)
    * [Configuration defaults and behavior](#configuration-defaults-and-behavior)

* [How it works](#how-it-works)

* [ToC generation](#toc-generation)

  * [ToC parameters](#toc-parameters)

* [Roadmap](#roadmap)

<!-- TOC END -->

## Badges and stuff

### Info

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![tested with jest](https://img.shields.io/badge/tested_with-jest-99424f.svg)](https://github.com/facebook/jest)

### Status

[![Dependencies freshness](https://img.shields.io/depfu/quilicicf/markdown-formatter)](https://depfu.com/repos/github/quilicicf/markdown-formatter)
[![Known Vulnerabilities](https://snyk.io/test/github/quilicicf/markdown-formatter/badge.svg)](https://snyk.io/test/github/quilicicf/markdown-formatter)
[![Build status](https://travis-ci.org/quilicicf/markdown-formatter.svg?branch=master)](https://travis-ci.org/quilicicf/markdown-formatter/builds)

## What it is

This formatter takes a markdown file and applies formatting rules to it.

It can also [add a ToC in your document](#toc-generation).

It is supposed to be used as a formatter for your markdown. Feel free to plug it to your favorite editor.

There are already plugins for [Atom](https://atom.io/packages/markdown-spec-formatter) and [VSCode](https://marketplace.visualstudio.com/items?itemName=quilicicf.markdown-spec-formatter).

> Note: obviously, this doc is formatted with markdown-formatter. Look at npm script `format:readme` in `package.json`.

## Use it

### CLI

```shell
$ npm install -g @quilicicf/markdown-formatter
$ markdown-format --content '**Toto**'
> __Toto__
$
```

#### Options

|      Option     | Alias | Type    | Description                                                                                                            |
| :-------------: | :---: | ------- | ---------------------------------------------------------------------------------------------------------------------- |
|   __content__   |   c   | String  | Markdown string to format. Mutually exclusive with `file`                                                              |
|     __file__    |   f   | String  | File path to Markdown file to format. Mutually exclusive with `content`                                                |
| __output-file__ |   o   | String  | When specified, creates/overwrites a file with the formatted markdown                                                  |
|   __replace__   |   r   | Boolean | Replaces the `file` content in-place. Mutually exclusive with `content` & `output-file`. Only valid when `file` is set |

### API

Both methods return a [VFile](https://github.com/vfile/vfile#api).

#### formatFromString

##### Usage

```js
const { formatFromString } = require('@quilicicf/markdown-formatter');

const main = async () => {
  const { contents, messages } = await formatFromString(
    '**Toto**', // Markdown string
    { watermark: 'top' }, // Markdown-formatter options
    { gfm: false }, // Stringify options
  );
  process.stdout.write(`Formatted from string:\n${contents}\n`);
  process.stdout.write(`With messages:\n${messages}\n`);
}

main();
```

##### Options

|           Parameter          |  Type  | Description                                                                                                                                                                                                                   |
| :--------------------------: | :----: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|          __content__         | String | Markdown string to format                                                                                                                                                                                                     |
| __markdownFormatterOptions__ | Object | [Markdown formatter options](./index.d.ts). Don't pass it or pass `{}` to use the [defaults](#configuration-defaults-and-behavior).                                                                                           |
|     __stringifyOptions__     | Object | [Stringification options](https://github.com/remarkjs/remark/tree/master/packages/remark-stringify#api) passed to `remark-stringify`. Don't pass it or pass `{}` to use the [defaults](#configuration-defaults-and-behavior). |

#### formatFromFile

##### Usage

```js
const { formatFromFile } = require('@quilicicf/markdown-formatter');

const main = async () => {
  const { contents } = await formatFromFile(
    filePath, // Markdown string
    { watermark: 'top' }, // Markdown-formatter options
    { gfm: false }, // Stringify options
  );
  process.stdout.write(`Formatted from file:\n${contents}\n`);
}

main();
```

##### Options

|           Parameter          |  Type  | Description                                                                                                                                                                                                                   |
| :--------------------------: | :----: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|         __filePath__         | String | Path to markdown file to format                                                                                                                                                                                               |
| __markdownFormatterOptions__ | Object | [Markdown formatter options](./index.d.ts). Don't pass it or pass `{}` to use the [defaults](#configuration-defaults-and-behavior).                                                                                           |
|     __stringifyOptions__     | Object | [Stringification options](https://github.com/remarkjs/remark/tree/master/packages/remark-stringify#api) passed to `remark-stringify`. Don't pass it or pass `{}` to use the [defaults](#configuration-defaults-and-behavior). |

#### Configuration defaults and behavior

Defaults for `markdownFormatterOptions` and `stringifyOptions` are available in the [constants file](./lib/constants.js).

For `stringifyOptions`, any attribute not present in the constants file defaults to the default value from the [remark-stringify plugin](https://github.com/remarkjs/remark/tree/master/packages/remark-stringify#api).

## How it works

It uses [remark](https://www.npmjs.com/package/remark) to parse the markdown and generate an AST.

Then [remark-stringify](https://www.npmjs.com/package/remark-stringify) to re-generate the string from the AST and apply the formatting rules to it.

Additionally, [mdast-util-toc](https://www.npmjs.com/package/mdast-util-toc) is used to generate a ToC.

## ToC generation

The ToC is inserted in the HTML comments described below and can be configured with the options also examplified.

```markdown
<!-- TOC START min:2 max:4 -->

> Anything between those two HTML comments will be replaced by the auto-generated ToC.
> The TOC parameters are optional, see default values in the table below

<!-- TOC END -->
```

### ToC parameters

| Name    | Accepted values          | Default value | Description                                                 |
| ------- | ------------------------ | :-----------: | ----------------------------------------------------------- |
| __min__ | Any number between 1 & 6 |       2       | The minimum level of headings that should appear in the ToC |
| __max__ | Any number between 1 & 6 |       4       | The maximum level of headings that should appear in the ToC |

## Roadmap

* [x] Create atom formatter
* [x] Create IntelliJ formatter
* [ ] Add dot graphs capabilities?
