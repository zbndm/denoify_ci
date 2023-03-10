
> ⚠️ THIS PROJECT IS NO LONGER MAINTAINED: Copy the CI setup of [tsafe](https://github.com/garronej/tsafe) instead.  



<p align="center">
    <img src="https://user-images.githubusercontent.com/6702424/82036935-c52a3480-96a1-11ea-9794-e982a23e5612.png">  
</p>
<p align="center">
    <i> A template to assist you in creating and publishing modules that will run everywhere: node, deno and the browser.  </i>
    <br>
    <br>
</p>

✅ NEW: `yarn` support  You are now free to use yarn instead of `npm` if you'd like to.

# Presentation 

This template automates the boring and tedious tasks of:
- Filling up the ``package.json``
- Setting up Typescript and [Denoify](https://github.com/garronej/denoify).
- Writing a [README.md](https://github.com/garronej/denoify_ci/blob/dev/README.template.md) with decent presentation and instructions on how to install/import your module.
- Testing on multiple Node and Deno version before publishing.
- Maintaining a CHANGELOG.
- Publishing on NPM and [deno.land/x](https://deno.land/x), creating corresponding GitHub TAGs.

Besides, good stuff that comes with using this template:
- No source files are tracked on the default branch.
- Shorter specific file import path.  
  ``import {...} from "my_module/theFile"`` instead of the usual
  ``import {...} from "my_module/dist/theFile"`` 
- CDN distribution for importing from ``.html`` files with a ``<script>`` tag.
- When your users hit *"Go to Definition"* they get redirected to the actual ``.ts`` source file instead of the ``.d.ts``.
  ( Feature disabled by default, refer to [instructions](#enabling-go-to-definition-to-redirect-to-the-source-ts-file) on how to enable it ).  
- Eslint and Prettifier are automatically run against files staged for commit. ( You can [disable](#disable-linting-and-formatting) this feature. )

# Table of content

- [Presentation](#presentation)
- [Table of content](#table-of-content)
- [How to use](#how-to-use)
  - [Fork it ( click use the template )](#fork-it--click-use-the-template-)
  - [Enable automatic publishing](#enable-automatic-publishing)
- [Few things you need to be aware of before getting started](#few-things-you-need-to-be-aware-of-before-getting-started)
- [Customization](#customization)
  - [Changing the directory structure](#changing-the-directory-structure)
  - [Enabling "Go to Definition" to redirect to the source ``.ts`` file](#enabling-go-to-definition-to-redirect-to-the-source-ts-file)
  - [Swipe the image in the ``README.md``](#swipe-the-image-in-the-readmemd)
  - [Disable linting and formatting](#disable-linting-and-formatting)
    - [Disable Prettier](#disable-prettier)
    - [Disable Eslint and Prettier altogether](#disable-eslint-and-prettier-altogether)
  - [Disable CDN build](#disable-cdn-build)
    - [Completely disable](#completely-disable)
    - [Only disable ES Module build ( ``dist/zz_esm/*`` )](#only-disable-es-module-build--distzz_esm-)
  - [Safely removable dev dependencies](#safely-removable-dev-dependencies)
  - [Customizing the Badges](#customizing-the-badges)
- [Accessing files on the disk.](#accessing-files-on-the-disk)
- [The automatically updated ``CHANGELOG.md``](#the-automatically-updated-changelogmd)
- [Video demo](#video-demo)
- [Examples of auto-generated readme](#examples-of-auto-generated-readme)
- [Creating a documentation website for your project](#creating-a-documentation-website-for-your-project)
- [Creating a landing page for your project](#creating-a-landing-page-for-your-project)


# How to use

## Fork it ( click use the template )

- Click on ![image](https://user-images.githubusercontent.com/6702424/98155461-92395e80-1ed6-11eb-93b2-98c64453043f.png)
- The repo name you will choose will be used as a module name for NPM and [deno.land/x](https://deno.land/x) so:
  - Make sure to use low dashes ( ``_`` ) instead of ( ``-`` ) to comply with deno naming standard.
  - Be sure it makes for a valid NPM module name.
  - Check if there is not already a NPM module named like that.
  - Check if there is not already a [deno.land/x](https://deno.land/x) module named like that.
- The description you provide will be the one used on NPM and deno.land ( you can change it later )

Once you've done that a GitHub action workflow will set up the ``README.md`` and the ``package.json`` 
 for you, wait a couple of minutes for it to complete ( a bot will push ). You can follow the job advancement in the "Action" tab.
(**warning** please read [this](#few-things-you-need-to-be-aware-of-before-getting-started))

Each time you will push changes ``npm run test:node`` and ``npm run test:deno`` will be run on remote docker 
containers against multiple Node and Deno versions, if everything passes you will get a green ``ci`` badges 
in your readme.

## Enable automatic publishing

Once you are ready to make your package available on NPM and deno.land/x you 
will need to provide two tokens so that the workflow can publish on your behalf:

Go to repository ``Settings`` tab, then ``Secrets`` you will need to add two new secrets:
- ``NPM_TOKEN``, you NPM authorization token.
- ``PAT``, GitHub **P**ersonal **A**ccess **T**oken with the **repo** and **write:repo_hook** authorizations. [link](https://github.com/settings/tokens)

To trigger publishing edit the ``package.json`` ``version`` field ( ``0.0.0``-> ``0.0.1`` for example) then push changes... that's all !
The publishing will actually be performed only if ``npm test`` passes.  

After a few minutes ( assuming the module name was still available ) you should be able to find your
module on [NPM](https://www.npmjs.com) and [deno.land/x](https://deno.land/x).

# Few things you need to be aware of before getting started

- [Denoify](https://github.com/garronej/denoify), the build tool that enable to support Deno, is still
very new, depending on what you have in mind it might require a lot of extra work to get your code 
to comply with the requirements it sets.  
If you are interested by the automation that this template features but don't care bout Deno support checkout [ts_ci](https://github.com/garronej/ts_ci).
- If you are not familiar with how Denoify works you might want to have a look at [this guide](https://github.com/garronej/my_dummy_npm_and_deno_module) first.
- You probably want to "Use this template" ( the green button ) instead of forking the repo.  
- The files to include in the NPM bundle are cherry-picked using the ``package.json`` ``files`` field.  
  If you don't want to bother and includes everything just remove the ``files`` field from the ``package.json``.
- Remember, when using ``fs`` that there is no `node_modules` directory in Deno. [Details](#accessing-files-on-the-disk).
- The template does not support ``.npmignore`` ( it uses ``package.json`` ``files`` which is [safer](https://medium.com/@jdxcode/for-the-love-of-god-dont-use-npmignore-f93c08909d8d) ).
- The template does not support ``.npmrc``.
- Unlike GitHub and NPM, [deno.land/x](https://deno.land/x) will not display HTML bits of your `README.md`.

# Customization

## Changing the directory structure

<details>
  <summary>Click to expand</summary>

All your source files must remain inside the ``src`` dir, you can change how things are organized inside the source directory
but don't forget to update your ``package.json`` ``main`` and ``type`` and ``tsconfig.esm.json`` ``include`` field when appropriate.

</details>

## Enabling "Go to Definition" to redirect to the source ``.ts`` file

<details>
  <summary>Click to expand</summary>

There is no denying that it is more convenient when clicking "Go To Definition" to get redirected to 
a file ``.ts`` file rather than to a ``.d.ts``.  

To enable this feature simply point to the ``package.json``'s ``types`` filed to the ``main``'s source
file instead the type definition file ``.d.ts``.  

For example you would replace:

```json
{
  "main": "./dist/index.js",
  "types": "./dist/index.d.ts",
}
```

by:

```json
{
  "main": "./dist/index.js",
  "types": "./src/index.ts",
}
```

Enabling this feature comes at a cost though. Be aware that if you use [optional chaining](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#optional-chaining) or [nullish coalescing](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#nullish-coalescing) for example, your module will only be importable
in projects using typescript 3.7 or newer ( version that introduces theses features ).  
It is important to keep your project compatible with older TS version because  
- You don't want to force your users to update the typescript version they use in their project,
  updating typescript might break some other things in their code. 
- In certain environments updating TypeScript is not an option. Take [Stackblitz](https://stackblitz.com) 
  for example.

</details>

## Swipe the image in the ``README.md``

<details>
  <summary>Click to expand</summary>

A good way to host your repo image is to open an issue named ASSET in your project, close it, create a comment, drag and drop the picture you want to use and that's it. You have a link that you can replace in the ``README.md``.  
While you are at it submit this image as *social preview* in your repos GitHub page's settings so that when you share on
Twitter or Reddit you don't get your GitHub profile picture to show up.

</details>

## Disable linting and formatting

### Disable Prettier

<details>
  <summary>Click to expand</summary>

[Prettier](https://prettier.io) is opinionated, it is OK to want to break free from it.

Remove these ``package.json``'s ``scripts``:  
- ``_format``
- ``format``
- ``format:check``

Remove these ``package.json``'s ``devDependencies``:  
- ``prettier``
- ``eslint-config-prettier``  

In the ``package.json``'s ``lint-staged`` field remove ``"*.{ts,tsx,json,md}": [ "prettier --write" ]``  

From ``.eslintrc.js``, remove the line: ``"prettier/@typescript-eslint",``.  

Delete these files:  
- ``.prettierignore``
- ``.prettierrc.json``  

In ``.github/workflows/ci.yaml`` remove the line ``npm run format:check`` from the ``test_lint`` job.  

</details>

### Disable Eslint and Prettier altogether

<details>
  <summary>Click to expand</summary>

Remove these ``package.json``'s ``scripts``:  

- ``_format``
- ``format``
- ``format:check``
- ``lint:check``
- ``lint``

Remove these ``package.json``'s ``devDependencies``:  
- ``prettier``
- ``eslint-config-prettier``  
- ``eslint``
- ``@typescript-eslint/parser``
- ``@typescript-eslint/eslint-plugin``
- ``husky``

Remove the  ``lint-staged`` and ``husky`` fields from the ``package.json``.  

Delete these files:  
- ``.prettierignore``
- ``.prettierrc.json``  
- ``.eslintignore``
- ``.eslintrc.js``

In ``.github/workflows/ci.yaml`` remove the ``test_lint`` job and the two lines ``needs: test_lint``.  

</details>

## Disable CDN build  

### Completely disable  

<details>
  <summary>Click to expand</summary>

If your project does not target the browser or if you are not interested in offering CDN distribution:

- Remove all ``cdn:*`` npm scripts and ``npm run cdn`` from the `build` script ( in ``package.json`` ).
- Remove ``./tsconfig.esm.json``
- Remove ``simplifyify`` and ``terser`` from dev dependencies.

</details>

### Only disable ES Module build ( ``dist/zz_esm/*`` )  

<details>
  <summary>Click to expand</summary>

If ``npm run build`` fail because ``tsc -p tsconfig.esm.json`` gives errors you may want to remove the ESM
build but keep the ``bundle.js`` and ``bundle.min.js``. To do that:

In ``package.json`` replace theses ``scripts``:  

```json
{
  "cdn:bundle:.js": "simplifyify dist/index.js -s #{REPO_NAME}# -o dist/bundle.js --debug --bundle",
  "cdn:bundle:.min.js": "terser dist/bundle.js -cmo dist/bundle.min.js",
  "cdn:bundle": "npm run cdn:bundle:.js && npm run cdn:bundle:.min.js",
  "cdn:esm": "tsc -p tsconfig.esm.json",
  "cdn": "npm run cdn:bundle && npm run cdn:esm",
}
```

By theses ones:

```json
{
  "cdn:.js": "simplifyify dist/index.js -s #{REPO_NAME}# -o dist/bundle.js --debug --bundle",
  "cdn:.min.js": "terser dist/bundle.js -cmo dist/bundle.min.js",
  "cdn": "npm run cdn:.js && npm run cdn:.min.js",
}
```

Remove ``tsconfig.esm.json``. ( file at the root of the project )  

Edit the ``README.md`` to remove instructions about how to 
import as ES module.

</details>

## Safely removable dev dependencies

<details>
  <summary>Click to expand</summary>

Dependencies that you can remove from the ``package.json`` if you don't use them:

- ``evt``
- ``@types/node``

</details>

## Customizing the Badges

<details>
  <summary>Click to expand</summary>

You can use [shields.io](https://shields.io) to create badges on metrics you would like to showcase.
  
</details>


# Accessing files on the disk.

Keep in mind that in Deno there is no ``node_modules`` sitting on the disk at runtime.  

<details>
  <summary>Click to expand</summary>

Let's assume for example that you would like to load a ``database.json`` file located 
at the root of your project. You would write something like this:  

``src/index.ts``
```typescript
import * as fs from "fs";
import * as path from "path";
import { TextDecoder } from "util";

export function getDatabase(): Record<string,any> {
    return JSON.parse(
        fs.readFileSync(
            path.join(
                __dirname,
                "..", "database.json"
            ),
            "utf8"
        ) as string
    );
}
```

This will work on both Node and Deno when you run your tests but once 
your module published this won’t work on Deno anymore for the same reason 
it won’t work in the Browser, the ``database.json`` file is present 
on the disk at runtime.  

</details>

# The automatically updated ``CHANGELOG.md``

Starting from the second release, a ``CHANGELOG.md`` will be created at the root of the repo.

*Example:*  
![image](https://user-images.githubusercontent.com/6702424/82747884-c47a5800-9d9d-11ea-8f3b-22df03352e54.png)

The ``CHANGELOG.md`` is built from the commits messages since last release.

Are NOT included in the ``CHANGELOG.md``:
- The commit messages that includes the word "changelog" ( non-case sensitive ). 
- The commit messages that start with "Merge branch ".
- The commit messages that with "GitBook: "

*The GitHub release will point to a freezed version of the ``CHANGELOG.md``*:  
![image](https://user-images.githubusercontent.com/6702424/82748469-6439e500-9da2-11ea-8552-ea9b7322dfa7.png)

# Video demo

This is a video demo is showcasing [ts_ci](https://github.com/garronej/ts_ci), a similar template repo but without the Deno support.

[![Watch the video](https://user-images.githubusercontent.com/6702424/82117367-c32ea700-976f-11ea-93f9-ec056aebc528.png)](https://youtu.be/Q5t-yP2PvPA)

# Examples of auto-generated readme

![npmjs com-2](https://user-images.githubusercontent.com/6702424/82423673-663f3380-9a84-11ea-8b78-c64215851f00.jpg)

# Creating a documentation website for your project

I recommend [GitBook](https://www.gitbook.com), It enables you to write your documentation in markdown from their 
website and get the markdown files synchronized with your repo.
They will provide you with a nice website for which you can customize the domain name.  
All this is covered by their free tier.  

Example: 
- [repo](https://github.com/garronej/evt)
- [GitBook documentation website](https://docs.evt.land)

I advise you to have a special directory at the root of your project where the markdown documentation files
are stored. It is configured by placing a ``.gitbook.yaml`` file at the root of the repo containing, for example:
``root: ./docs/``

Do not hesitate to request free access to premium features. Open source projects are eligible!  

PS: I am not affiliated with GitBook in any way.

# Creating a landing page for your project

Beside the documentation website, you might want to have a catchy landing page to share on social networks.  
You can use [GitHub pages](https://pages.github.com) to host it. 

If you like the landing page of EVT, [evt.land](http://evt.land), you can fork the [repo](https://github.com/garronej/evt.land) and adapt it for your module.  

To produce high quality GIF from screen recording that remain relatively small checkout the wonderful [Gifski](https://gif.ski) from [Sindre Sorhus](https://github.com/sindresorhus).

You'll just have to go to settings and enable Pages.

![image](https://user-images.githubusercontent.com/6702424/82155402-0aeb2680-9875-11ea-9159-f6167ee2928e.png)

And update your DNS: 

![image](https://user-images.githubusercontent.com/6702424/82155473-7e8d3380-9875-11ea-9bba-115cbb3ef162.png)

I personally use [Hurricane Electric](https://dns.he.net) free DNS servers because they support a lot of record types.
However, if your DNS provider does not support ``ALIAS``, you can use ``A`` records and manually enter the IP of GitHub servers.
I let you consult the [GitHub Pages Documentation](https://help.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain). 
