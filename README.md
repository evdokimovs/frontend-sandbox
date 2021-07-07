Frontend sandbox
===============

Using [Jason] library and [Medea] WebRTC media server create an application for audio/video calls between two users.

[Design](https://www.figma.com/file/3x98kN1De6US3ek9a9WJof/Frontend-Sandbox)
| [Prototype](https://www.figma.com/proto/3x98kN1De6US3ek9a9WJof/Frontend-Sandbox)

> This is a [GitHub template][9] repository, so you need to click on `Use this template` button above and do all the development in your own repository which uses this one as template only.




## Required features

1. Display video and play audio of a partner participant.
2. Publish video from the frontal camera of a device.
3. Publishing user's audio.
4. Preview of the published video.
5. Ability to enable/disabled published video/audio.




## Implementation requirements

1. Code should be documented with [JSDoc].
2. Code should be covered with unit tests.
3. [E2E (end-to-end) tests][3] should cover all the required features.




## [WebRTC signaling][1] server

For [signaling][1] between client you should use [Medea] media server with a [Jason] library.

Read [Control API RFC][18] and [Client API RFC][19] for better understanding of how it works. You don't need to use [Control API][18] directly, instead you can use following static Control API spec:

```yaml
kind: Room
id: call
spec:
  pipeline:
    Alice:
      kind: Member
      credentials:
        plain: helloworld
      spec:
        pipeline:
          publish:
            kind: WebRtcPublishEndpoint
            spec:
              p2p: Always
          play-responder:
            kind: WebRtcPlayEndpoint
            spec:
              src: "local://call/Bob/publish"
    Bob:
      kind: Member
      credentials:
        plain: helloworld
      spec:
        pipeline:
          publish:
            kind: WebRtcPublishEndpoint
            spec:
              p2p: Always
          play-caller:
            kind: WebRtcPlayEndpoint
            spec:
              src: "local://call/Alice/publish"
```

This spec will be automatically created on your [Medea] instance deployed on [Heroku].


### Deploying on [Heroku]

1. Create account on [Heroku] (if you don't have one).
2. Copy [Heroku] API key from the [account page][22].
3. Go to [Actions Secrets][23] settings in your GitHub repository.
4. Add the following repository keys:
    - `HEROKU_API_KEY` - API key which you copied at step 2;
    - `HEROKU_EMAIL` - email with which you registered on [Heroku].
5. Go to ['Deploy Medea media server to Heroku'][24] GitHub workflow.
6. Run workflow on `master` branch.

Now your instance of a [Medea] media server can be accessed at `wss://frontend-sandbox-{{ YOUR GITHUB USERNAME }}.herokuapp.com/ws`.


### Useful links

- [Medea source code][Medea]
- [Jason source code][Jason]
- [Jason documentation](https://docs.rs/medea-jason)
- [Medea Control API mock server endpoints documentation and source code](https://github.com/instrumentisto/medea/tree/master/mock/control-api)




## Enable [GitHub Pages] in your repository

1. Go to [Pages][21] settings in your repository
2. Set [GitHub Pages] source branch to `gh-pages`

Now your application will be built and published to the [GitHub Pages] automatically on every push to `master` branch.




## Final demonstration

Firstly, you should demonstrate that application can make calls between users and disabling/enabling audio/video works correctly.

At the end, your application (published to [GitHub Pages]) should be able to make video call with a reviewer.




## Required assets

All the assets required for this application are located in the `assets/` directory.




## Final design of application

Final design of the implemented application may vary from the provided one. The provided design aims only to explain the expected result better.




## Useful links

- [Basics of WebRTC](https://www.html5rocks.com/en/tutorials/webrtc/basics)
- [Basic WebRTC glossary](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Protocols)
- [Basics of WebRTC signaling](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Connectivity)
- [WebRTC glossary](https://webrtcglossary.com)
- [Medea Jason usage example](https://github.com/instrumentisto/medea/tree/master/jason/e2e-demo)
- [Dead simple WebRTC example written with JS](https://shanetully.com/2014/09/a-dead-simple-webrtc-example)




## Repository requirements


### Files

Repository __must NOT contain__ (so have them [Git-ignored] to avoid accidents):
- __configuration__ files __of__ developer's __local toolchain__ (unless this configuration is suitable for all project developers);
- __compilation/build results/artifacts__ of source code;
- any __caches or temporary files__;
- __configuration__ files __for running__ application (except examples or Dockerized development environment configurations which are the same for all project developers).

__For keeping an empty directory__ in a repository __use the `.gitkeep` file__ inside that directory.

#### Naming

__Start directory with `.`__ if it contains some __temporary files which do not require direct manipulations__ and are going to be omitted by tools (caches, temp files, etc.). This is a quite common practice (see `.git/`, `.idea/`, `.gradle/`, etc.).  
Also, __all temporary cache files__ must be __placed inside a `.cache/`__ top-level directory of the repository, unless this is impossible for somewhat reasons.

__To emphasize toolchain directories__ (ones which do not contain project sources itself, but rather contain files of a project toolchain) their __name may be started with `_`__, which will make them to "bubble-up" in a repository source tree, so will allow easily to distinguish them from actual project sources (both for humans and tools).


### Branches and tags

Every repository contains the following branches:

- `master` - __mainline version__ of the project. Any new project release is usually created from this branch. Developing directly in this branch is forbidden. It __accepts new changes via PRs (pull requests)__.

Any other possible branches and tags can be created and used by developers as they need.

#### Branch naming

[Git] branch name __must__ meet the following requirements:
- consist of __English words__;  
    👍 `fix-tests-failure`  
    🚫 `fix-defectum-probat`
- use __only dashes to separate words__;  
    👍 `fix-tests-failure`  
    🚫 `fix_tests_failure`
- use __[imperative mood][2] for verbs__;  
    👍 `fix-tests-failure`  
    🚫 `fixes-tests-failure`
- __start with the issue number__ when branch is related to some issue (but __DO NOT use PR (pull request) numbers__);  
    👍 `23-fix-tests-failure`  
    🚫 `fix-tests-failure`
- __reflect the meaning of branch changes__, not the initial problem.    
    👍 `23-fix-tests-failure`  
    🚫 `23-problem-with-failing-tests`


### Commits

Every __commit message must contain a short description__ of its changes that meet the following requirements:
- be __on English__ (no other language is allowed);
- __start with a capital letter__;
- has __no punctuation symbols at the end__ (like `;`, `:` or `.`);
- use __[imperative mood][2] for verbs__ (as if you are commanding someone: `Fix`, `Add`, `Change` instead of `Fixes`, `Added`, `Changing`);
- use __marked list for multiple changes__, prepended by __one summary line__ and __one blank line__, where each __list item__ must:
    - __start with a lowercase letter__;
    - has __no punctuation symbols at the end__.

##### 👍 Single-line commit message example

```
Update Employee salary algorithm
```

##### 👍 Multiple-line commit message example

```
Implement employees salary and ajax queries

- update Employee salary algorithm
- remove unused files from public/images/ dir
- implement ajax queries for /settings page
```

##### 🚫 Wrong commit message examples

- Summary line starts with a lowercase letter:

    ```
    update Employee salary algorithm
    ```

- Verb is not in the [imperative mood][2]:

    ```
    Updates Employee salary algorithm
    ```

- Unnecessary punctuation is present:

    ```
    Update Employee salary algorithm.
    ```

    ```
    Implement employees salary and ajax queries:
    
    - update Employee salary algorithm;
    - remove unused files from public/images/ dir.
    ```

- Missing blank line between the summary line and the marked list:

    ```
    Implement employees salary and ajax queries
    - update Employee salary algorithm
    - remove unused files from public/images/ dir
    ```

- Marked list is indented:

    ```
    Implement employees salary and ajax queries
    
      - update Employee salary algorithm
      - remove unused files from public/images/ dir
    ```

- Marked list items start with a capital letter:

    ```
    Implement employees salary and ajax queries
    
    - Update Employee salary algorithm
    - Remove unused files from public/images/ dir
    ```


### FCM (final commit message)

FCM (final commit message) is a commit message of a pull request to a `master` branch.

As it will be saved in a repository history forever, it has __extra requirements__ that __must__ be met:
- __contain references__ to related PR;
- do __not contain__ any non-relative helper markers (like `[skip ci]`);

__Common commit messages__ which are not FCM __must NOT contain any references__, because references create crosslinks in mentioned PRs, which leads to spamming issues/PRs with unnecessary information. Only saved in history forever commits are allowed to create such crosslinks.

If а PR contains some __side changes__ which are not directly relevant to the task, then such changes __must be described as a marked list in the `Additionally:` section (separated by a blank line)__ of a FCM.

##### 👍 FCM examples

```
Implement employees salary and ajax queries (#43, #54)

- update Employee salary algorithm
- remove unused files from public/images/ dir

Additionally:
- update Git ignoring rules for TOML files
```

##### 🚫 Wrong FCM examples

- Bad formatting of [references][103]:

    ```
    Implement employees salary and ajax queries(#43,#54)

    - update Employee salary algorithm
    - remove unused files from public/images/ dir
    ```

- Side changes are not separated:

    ```
    Implement employees salary and ajax queries (#43, #54)

    - update Employee salary algorithm
    - remove unused files from public/images/ dir
    - update Git ignoring rules for TOML files
    ```

- Bad formatting of side changes:

    ```
    Implement employees salary and ajax queries (#43, #54)

    - update Employee salary algorithm
    - remove unused files from public/images/ dir
    Additionally:
    - update Git ignoring rules for TOML files
    ```


### Merging

__All merges to the mainline__ project version (`master` branch) __must have an individual PR (pull request)__ and must be __done only in [fast-forward] manner__. This is required to keep mainline history linear, simple and clear.

To achieve [fast-forward merge][fast-forward], __all branch commits__ (which doesn't exist in mainline) __must be squashed and rebased onto the latest mainline commit__. Notable moments are:
- Before rebase __do not forget to merge your branch with latest mainline branch updates__, otherwise rebase result can break changes.

#### Squash merging steps

##### Using GitHub UI

Performing squash merge correctly can be quite tricky when doing manually. To avoid complexity and mistakes in a day-to-day routine the [GitHub UI squash merging][13] is the __most preferred way__ for merging and a __developer should use it whenever it's possible__.

0. Merge with latest `master` branch.

1. Click on `Squash and merge` button.

2. Paste first line of FCM to title field.

3. Paste FCM without first line to body field.

4. Click `Confirm squash and merge`.

- __First line__ of FCM must go __as a title__ of the squash commit and __everything after as a message__.

Squash merging via GitHub UI also preserves the whole branch commits history in the PR, which is good for history purposes.


### Pushing

Developer __must push all his changes__ to the remote __at the end of his working day__. This both prevents from accidental work losses and helps a lead to track developer's progress.




## Project requirements

All features of application should be added with PRs. __Direct push to `master` is forbidden.__


### Pull requests

PRs (pull requests) are created to make changes in the repository and to solve some problem (fix a bug, implement a task, provide an improvement, etc).

PR __must contain related changes only__. Any __other unrelated changes__ of repository must be done __via separate PR__. This rule keeps project history clear and unambiguous.

PR __name must__:
- __shortly and clearly describe its meaning__;
- __contain `Draft: ` prefix__ until PR is merged or closed.

Not merged or closed PRs should be in [draft mode][11].

PR __description must contain details of the solution__ (background/summary, solution description, notable moments, etc).

Project have [PR template][12] which standardize PRs. Developer must __use [PR template][12] whenever it's possible__.  
If there is no template for some rare case, then the PR must be formatted in the same manner as available templates.

PR cannot be assigned to nobody and __always must have an assigned developer__.

PR cannot go without any labels and __must have all required labels correctly applied__.


### Labels

Labels are used for issues/PRs classification, as they:
- reflect the current state of issue/PR;
- improve understanding of issue/PR, its purpose and application;
- provide advanced search of issues/PRs;
- allow to sum up statistics of how project is going on.

There are several label groups:
- Type labels declare what the current issue/PR actually represents. These labels are __mandatory__: each issue/PR must have at least one such label.
    - `feature` applies when something new is implemented (or is going to be implemented).
    - `enhancement` applies when changing of existing features is involved (improvement or bugfix).
    - `rollback` applies when some existing changes are going to be rolled back.
- `k::` labels describe what the current issue/PR is relevant to and which project aspects are involved. These labels are __mandatory__: each issue/PR must have at least one such label.
    - `k::ui` applies to UI (user interface) and UX (user experience) changes. Use it when end-user are directly affected by this changes.
    - `k::api` applies to API (application interface) changes. Use it when you're changing application interfaces, like: HTTP API method parameters, library exported interfaces, command-line interfaces, etc.
    - `k::deploy` applies to changes that involve application deployment. Use it when you're changing the way application is deployed.
    - `k::design` applies to changes of application architecture and implementation design. Use it when you're changing architecture and algorithms.
    - `k::documentation` applies to changes of project documentation.
    - `k::logging` applies to changes in application logs.
    - `k::performance` applies to application performance related changes.
    - `k::refactor` applies to refactor changes of existing code.
    - `k::security` applies to application security related changes.
    - `k::testing` applies to changes of project tests.
    - `k::toolchain` applies to changes of project toolchain.




## Code style

## JavaScript

All JavaScript source code must follow [Google JavaScript Style Guide][5].

Any rules described here are in priority if they have conflicts with
Google JavaScript Style Guide.


### jQuery

__Name of variable__ that is __assigned to `jQuery`__ object (`$`)
__must start with `$` symbol__.
```js
// WRONG
var elem = $('#header');

// CORRECT
var $elem = $('#header');
```


### Methods chain

When wrapping methods chain the __`.` (invocation operator) must be placed
before method__ it invokes (unlike other operators).
```js
// WRONG
very().
    big().
    method().
    chain();

// CORRECT
very()
    .big()
    .method()
    .chain();
```

__Long methods chain__ must be __split to indented logical blocks__.  
For example, it's good to split method chain into logical blocks basing on
queried elements and performed operations.
```js
// WRONG
$('.sidebar .menu')
    .find('a')
    .removeClass('active')
    .filter('[href="' + pageAlias + '"]')
    .addClass('active');

// CORRECT
$('.sidebar .menu')
    .find('a')
        .removeClass('active')
    .filter('[href="' + pageAlias + '"]')
        .addClass('active');
```


### `var` block

__Each new variable__ must be __aligned to the first variable__ of `var` block,
but not indented.
```js
// WRONG
var $elem = $('#header'),
  isActive = $elem.hasClass('active'),
  num = 0;

// CORRECT
var $elem = $('#header'),
    isActive = $elem.hasClass('active'),
    num = 0;
```

If __expression is too long__ and must be wrapped, then its __wrapped lines
must be aligned__ (with their indents) to __position where expression starts__.
```js
// WRONG
var sum = 23 + 24 + 25 +
    26 + 27 + 28,
    ternary = some ?
    yes :
    no,
    isActive = $elem.find('.some')
    .hasClass('active'),
    num = veryLongFunctionNameWithResult(
    some, arguments, here);

// CORRECT
var sum = 23 + 24 + 25 +
          26 + 27 + 28,
    ternary = some ?
              yes :
              no,   
    isActive = $elem.find('.some')
                   .hasClass('active'),
    num = veryLongFunctionNameWithResult(
              some, arguments, here),
    prefferable = veryLongFunctionNameWithResult(some,
                                                 arguments, here);
```

The __exception is functions, arrays or object definition__. In this case
__alignment must be set to position of the first `var` block variable__.
```js
// WRONG
var myObject = {
                 my: 'property',
                 your: 'responsibility',
               },
    myArray = [
                'some',
                'values',
                'here'
              ],
    myFunc = function() {
               console.log('Hello darkness, my old friend, ' +
                           'I\'ve come to talk with you again.');
             };

// WRONG
var myObject = {
  my: 'property',
  your: 'responsibility',
},
    myArray = [
  'some',
  'values',
  'here'
],
    myFunc = function() {
  console.log('Hello darkness, my old friend, ' +
              'I\'ve come to talk with you again.');
};

// CORRECT
var myObject = {
      my: 'property',
      your: 'responsibility',
    },
    myArray = [
      'some',
      'values',
      'here'
    ],
    myFunc = function() {
      console.log('Hello darkness, my old friend, ' +
                  'I\'ve come to talk with you again.');
    };
```


### JSDoc

__JSDoc parameters and types__ must be described __in [common JSDoc
manner][6]__ despite the fact that [Google JavaScript Style Guide provides
its own format][10]. This is required because common IDEs and static analysis
tools do not play well with Google JavaScript Style Guide JSDoc declarations.




## Vue.js

All Vue components must follow [Vue.js Component Style Guide][14].

The guide is available on several languages, but __English is preferred__,
because it's always up-to-date the most.

Vue __components__ must be __separated into at least two `.vue` and `.ts`
files__ to support TypeScript linting and IDE integration
normally.




## TypeScript

All TypeScript source code must follow [TypeScript coding guidelines][15].
An official [handbook][16] is also a good starting point to read before
migrating from JavaScript to TypeScript.

Any __rules described here are in priority__ if they have conflicts with
TypeScript coding guidelines.

All TypeScript __sources__, __tests__ and __types__ must be __linted__ without
any warnings by [TSLint][17] (which defines rules described here along with recommended TSLint rules).


### String and Quotes

Using raw strings (__single-quoted__) __is preferable over double-quoted__
strings. Double-quoted strings can be used, when it's necessary to quote
something in a string, that is already single-quoted, or in a documentation, for
example.
```typescript
// CORRECT
import 'Vue' from 'vue';

/**
 * Test class, that has "message" property.
 */
export default class Test {
    private message: string = 'Hello world!';
}


// WRONG
import "Vue" from "vue";

/**
 * Test class, that has 'message' property
 */
export default class Test {
    private message: string = "Hello world!";
}
```


### Trailing comma

__Trailing comma is mandatory in multi-line declarations__
(array/object literals, destructuring assignments, function typings,
named imports/exports and function parameters)
and __forbidden in single-line__ declarations.
```typescript
// CORRECT
const store = new Store({
    actions,
    getters,
});
const multiLineArray = [
    1,
    2,
    3,
];
const singleLineArray = [1,2,3];

// WRONG
const store = new Store({
    actions,
    getters
});
const multiLineArray = [
    1,
    2,
    3
];
```


### `import` statement

All `import` statements must be __divided into the following ordered
sections__:
1. imports of third-party libraries modules;
2. imports of application itself modules;
3. imports of components (for UI applications);
4. imports of application itself test modules (required for specs/tests).

A __blank line__ is mandatory __between these sections__.
All import sections __must be separated__ from the rest of code by __2 blank
lines__.

__Inside__ each section imports must be __sorted in alphabetical__ order
__by__ their __path__ (not imported name).

```typescript
// CORRECT
import Vue from 'vue';
import Component from 'vue-class-component';
import { Store } from 'vuex';

import HMRApi from 'HMRApi';
import RootState from 'store/root/state';

import BottomMenu from 'components/bottom-menu/BottomMenu.vue';
import MenuSidebar from 'components/menu-sidebar/MenuSidebar.vue';
import TopMenu from 'components/top-menu/TopMenu.vue';
import QuickAccessSidebar from 'components/quick-access-sidebar/QuickAccessSidebar.vue';

import Helper from 'unit/Helper';


class App {}


// WRONG
import HMRApi from 'HMRApi';
import RootState from 'store/root/state';
import Vue from 'vue';
import Component from 'vue-class-component';
import { Store } from 'vuex';
import Helper from 'unit/Helper';
import BottomMenu from 'components/bottom-menu/BottomMenu.vue';
import MenuSidebar from 'components/menu-sidebar/MenuSidebar.vue';
import TopMenu from 'components/top-menu/TopMenu.vue';
import QuickAccessSidebar from 'components/quick-access-sidebar/QuickAccessSidebar.vue';

class App {}
```

`import` statement __must not be wrapped__, even if it overlaps max allowed
line length.  
__Each import__ must take exactly __one line__.

To disable TSLint warning about max line length, you can use
`// tslint:disable-line` comment at the end of the line.
```typescript
// CORRECT
import QuickAccessSidebar from 'components/quick-access-sidebar/QuickAccessSidebar.vue'; // tslint:disable-line

// WRONG
import QuickAccessSidebar from
    'components/quick-access-sidebar/QuickAccessSidebar.vue';
```


### Multiline methods arguments

When it comes to formatting multiline method arguments, the following rules
must be met:
- __each argument__ takes its __own line__ and is __indented__;
- the __closing parenthesis and__ the __opening brace__ take their __own line__.

```typescript
// CORRECT
private beforeEach(
    to: Route,
    from: Route,
    next: (to?: RawLocation | false | ((vm: Vue) => any) | void) => void,
) {
    store.dispatch(START_LOADING);
    next();
}

// WRONG
private beforeEach(to: Route, from: Route,
    next: (to?: RawLocation | false | ((vm: Vue) => any) | void) => void) {
    store.dispatch(START_LOADING);
    next();
}
```


### Specs

__Top-level sections__ (usually `describe`) in the specs (unit or e2e) must be
__separated by 2 blank lines__ between each other.

Lower-level sections (usually `it` statements) must be __separated by
1 blank line__.

```typescript
// CORRECT
describe('components/app/App.vue', () => {


    let app: Vue;
    let component: AppType;

    before(() => {
        return Helper.initApp(App).then((vm: Vue) => {
            app = vm;
            component = app.$children[0] as any;
        });
    });


    describe('preFetch()', () => {

        it('returns Promise', () => {
            expect(component.preFetch(component.$store))
                .to.be.a('Promise');
        });

    });


});


// WRONG
describe('components/app/App.vue', () => {

    let app: Vue;
    let component: AppType;

    before(() => {
        return Helper.initApp(App).then((vm: Vue) => {
            app = vm;
            component = app.$children[0] as any;
        });
    });

    describe('preFetch()', () => {
        it('returns Promise', () => {
            expect(component.preFetch(component.$store))
                .to.be.a('Promise');
        });
    });

});
```



### `.editorconfig` rules

Project contains [`.editorconfig` file][7] with both general and project-specific code style rules.

__Applying `.editorconfig` rules is mandatory.__

Make sure that your IDE supports `.editorconfig` rules applying. For JetBrains IDE the [EditorConfig plugin][8] may be used.





[1]: https://webrtcglossary.com/signaling/
[2]: https://en.wikipedia.org/wiki/Imperative_mood
[3]: https://www.browserstack.com/guide/end-to-end-testing
[4]: https://dashboard.heroku.com/account
[5]: https://google.github.io/styleguide/javascriptguide.xml
[6]: http://usejsdoc.org/tags-param.html
[7]: http://editorconfig.org
[8]: https://plugins.jetbrains.com/phpStorm/plugin/7294-editorconfig
[9]: https://github.blog/2019-06-06-generate-new-repositories-with-repository-templates
[10]: https://google.github.io/styleguide/javascriptguide.xml?showone=JavaScript_Types#JavaScript_Types
[11]: https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests#draft-pull-requests
[12]: https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/about-issue-and-pull-request-templates#pull-request-templates
[13]: https://docs.github.com/en/github/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges#squash-and-merge-your-pull-request-commits
[14]: https://pablohpsilva.github.io/vuejs-component-style-guide
[15]: https://github.com/Microsoft/TypeScript/wiki/Coding-guidelines
[16]: http://www.typescriptlang.org/docs/handbook/basic-types.html
[17]: https://palantir.github.io/tslint
[18]: https://github.com/instrumentisto/medea/blob/master/docs/rfc/0001-control-api.md
[19]: https://github.com/instrumentisto/medea/blob/master/docs/rfc/0002-webrtc-client-api.md
[20]: https://github.com/instrumentisto/medea/tree/master/mock/control-api
[21]: /../../settings/pages
[22]: https://dashboard.heroku.com/account
[23]: /../../settings/secrets/actions
[24]: /../../actions/workflows/deploy-medea.yml


[dartdoc]: https://dart.dev/tools/dartdoc
[dartfmt]: https://dart.dev/tools/dart-format
[Effective Dart]: https://www.dartlang.org/guides/language/effective-dart
[fast-forward]: https://ariya.io/2013/09/fast-forward-git-merge
[flutter_webrtc]: https://pub.dev/packages/flutter_webrtc
[Git]: https://git-scm.com
[Git-ignored]: https://git-scm.com/docs/gitignore
[Heroku]: https://www.heroku.com
[ICE]: https://webrtcglossary.com/ice
[Medea]: https://github.com/instrumentisto/medea
[Jason]: https://github.com/instrumentisto/medea/tree/master/jason
[GitHub Pages]: https://pages.github.com/
[JSDoc]: https://jsdoc.app/
[Heroku]: https://www.heroku.com
