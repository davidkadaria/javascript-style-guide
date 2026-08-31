# eslint-config-airbnb-base <sup>[![Version Badge][npm-version-svg]][package-url]</sup>

[![npm version](https://badge.fury.io/js/eslint-config-airbnb-base.svg)][package-url]

[![github actions][actions-image]][actions-url]
[![License][license-image]][license-url]
[![Downloads][downloads-image]][downloads-url]

This package provides Airbnb's base JS .eslintrc (without React plugins) as an extensible shared config.
ეს პაკეტი Airbnb-ის საბაზისო JS .eslintrc-ს (React-ის პლაგინების გარეშე) გაფართოებადი საზიარო კონფიგურაციის სახით გთავაზობთ.

## Usage
## გამოყენება

We export two ESLint configurations for your usage.
თქვენთვის ESLint-ის ორ კონფიგურაციას ვაექსპორტებთ.

### eslint-config-airbnb-base

Our default export contains all of our ESLint rules, including ECMAScript 6+. It requires `eslint` and `eslint-plugin-import`.
ჩვენი ნაგულისხმევი ექსპორტი ESLint-ის ჩვენს ყველა წესს შეიცავს, ECMAScript 6+-ის ჩათვლით. იგი მოითხოვს `eslint`-სა და `eslint-plugin-import`-ს.

1. Install the correct versions of each package, which are listed by the command:
1. დააინსტალირეთ თითოეული პაკეტის სწორი ვერსია; მათ შემდეგი ბრძანება ჩამოთვლის:

  ```sh
  npm info "eslint-config-airbnb-base@latest" peerDependencies
  ```

  If using **npm 5+**, use this shortcut
  თუკი **npm 5+**-ს იყენებთ, გამოიყენეთ ეს მალსახმობი

  ```sh
  npx install-peerdeps --dev eslint-config-airbnb-base
  ```

  If using **yarn**, you can also use the shortcut described above if you have npm 5+ installed on your machine, as the command will detect that you are using yarn and will act accordingly.
  თუკი **yarn**-ს იყენებთ, ზემოთ აღწერილი მალსახმობის გამოყენებაც შეგიძლიათ, თუკი თქვენს მანქანაზე npm 5+ გაქვთ დაინსტალირებული, რადგანაც ბრძანება აღმოაჩენს, რომ yarn-ს იყენებთ, და შესაბამისად იმოქმედებს.
  Otherwise, run `npm info "eslint-config-airbnb-base@latest" peerDependencies` to list the peer dependencies and versions, then run `yarn add --dev <dependency>@<version>` for each listed peer dependency.
  წინააღმდეგ შემთხვევაში, თანმხლები დამოკიდებულებებისა (*peer dependencies*) და ვერსიების ჩამოსათვლელად გაუშვით `npm info "eslint-config-airbnb-base@latest" peerDependencies`, შემდეგ კი თითოეული ჩამოთვლილი თანმხლები დამოკიდებულებისთვის გაუშვით `yarn add --dev <dependency>@<version>`.


  If using **npm < 5**, Linux/OSX users can run
  თუკი **npm < 5**-ს იყენებთ, Linux/OSX-ის მომხმარებლებს შეუძლიათ გაუშვან

  ```sh
  (
    export PKG=eslint-config-airbnb-base;
    npm info "$PKG@latest" peerDependencies --json | command sed 's/[\{\},]//g ; s/: /@/g' | xargs npm install --save-dev "$PKG@latest"
  )
  ```

  Which produces and runs a command like:
  რაც შემდეგნაირ ბრძანებას ქმნის და უშვებს:

  ```sh
    npm install --save-dev eslint-config-airbnb-base eslint@^#.#.# eslint-plugin-import@^#.#.#
  ```

  If using **npm < 5**, Windows users can either install all the peer dependencies manually, or use the [install-peerdeps](https://github.com/nathanhleung/install-peerdeps) cli tool.
  თუკი **npm < 5**-ს იყენებთ, Windows-ის მომხმარებლებს შეუძლიათ ან ყველა თანმხლები დამოკიდებულება ხელით დააინსტალირონ, ან [install-peerdeps](https://github.com/nathanhleung/install-peerdeps) cli-ხელსაწყო გამოიყენონ.

  ```sh
  npm install -g install-peerdeps
  install-peerdeps --dev eslint-config-airbnb-base
  ```

  The cli will produce and run a command like:
  cli შემდეგნაირ ბრძანებას შექმნის და გაუშვებს:

  ```sh
  npm install --save-dev eslint-config-airbnb-base eslint@^#.#.# eslint-plugin-import@^#.#.#
  ```

2. Add `"extends": "airbnb-base"` to your .eslintrc.
2. დაამატეთ `"extends": "airbnb-base"` თქვენს .eslintrc-ს.

> **Note**: ESLint only lints `.js` files by default.
> **შენიშვნა**: ESLint ნაგულისხმევად მხოლოდ `.js` ფაილებს ამოწმებს.

  If your project uses `.jsx` (or `.tsx` with TypeScript), you need to pass extensions to the CLI:
  თუკი თქვენი პროექტი `.jsx`-ს (ან TypeScript-თან ერთად `.tsx`-ს) იყენებს, CLI-ს გაფართოებები უნდა გადასცეთ:

  ```sh
  eslint . --ext .js, .jsx, .mjs
  ```

  Without this, JSX-related rules will not apply to `.jsx` files.
  ამის გარეშე JSX-თან დაკავშირებული წესები `.jsx` ფაილებზე არ გავრცელდება.

### eslint-config-airbnb-base/legacy

Lints ES5 and below. Requires `eslint` and `eslint-plugin-import`.
ამოწმებს ES5-სა და უფრო ძველ ვერსიებს. მოითხოვს `eslint`-სა და `eslint-plugin-import`-ს.

1. Install the correct versions of each package, which are listed by the command:
1. დააინსტალირეთ თითოეული პაკეტის სწორი ვერსია; მათ შემდეგი ბრძანება ჩამოთვლის:

  ```sh
  npm info "eslint-config-airbnb-base@latest" peerDependencies
  ```

  Linux/OSX users can run
  Linux/OSX-ის მომხმარებლებს შეუძლიათ გაუშვან
  ```sh
  (
    export PKG=eslint-config-airbnb-base;
    npm info "$PKG" peerDependencies --json | command sed 's/[\{\},]//g ; s/: /@/g' | xargs npm install --save-dev "$PKG"
  )
  ```

  Which produces and runs a command like:
  რაც შემდეგნაირ ბრძანებას ქმნის და უშვებს:

  ```sh
  npm install --save-dev eslint-config-airbnb-base eslint@^#.#.# eslint-plugin-import@^#.#.#
  ```

2. Add `"extends": "airbnb-base/legacy"` to your .eslintrc
2. დაამატეთ `"extends": "airbnb-base/legacy"` თქვენს .eslintrc-ს

See [Airbnb's overarching ESLint config](https://npmjs.com/eslint-config-airbnb), [Airbnb's JavaScript styleguide](https://github.com/airbnb/javascript), and the [ESlint config docs](https://eslint.org/docs/user-guide/configuring#extending-configuration-files) for more information.
დამატებითი ინფორმაციისათვის იხილეთ [Airbnb-ის ESLint-ის ზოგადი კონფიგურაცია](https://npmjs.com/eslint-config-airbnb), [Airbnb-ის JavaScript-ის სტილის სახელმძღვანელო](https://github.com/airbnb/javascript) და [ESlint-ის კონფიგურაციის დოკუმენტაცია](https://eslint.org/docs/user-guide/configuring#extending-configuration-files).

### eslint-config-airbnb-base/whitespace

This entry point only errors on whitespace rules and sets all other rules to warnings. View the list of whitespace rules [here](https://github.com/airbnb/javascript/blob/master/packages/eslint-config-airbnb-base/whitespace.js).
ეს შესასვლელი წერტილი შეცდომებს მხოლოდ ინტერვალების წესებზე აგდებს, ყველა დანარჩენ წესს კი გაფრთხილებებად აყენებს. ინტერვალების წესების სია იხილეთ [აქ](https://github.com/airbnb/javascript/blob/master/packages/eslint-config-airbnb-base/whitespace.js).

## Improving this config
## ამ კონფიგურაციის გაუმჯობესება

Consider adding test cases if you're making complicated rules changes, like anything involving regexes. Perhaps in a distant future, we could use literate programming to structure our README as test cases for our .eslintrc?
თუკი წესებში რთულ ცვლილებებს შეგაქვთ, მაგალითად, რეგულარულ გამოსახულებებთან დაკავშირებულს, სატესტო შემთხვევების დამატება განიხილეთ. იქნებ შორეულ მომავალში ჩვენი README ჩვენივე .eslintrc-ის სატესტო შემთხვევებად „წიგნიერი პროგრამირების“ (*literate programming*) მეშვეობით დაგვესტრუქტურირებინა?

You can run tests with `npm test`.
ტესტების გაშვება `npm test`-ით შეგიძლიათ.

You can make sure this module lints with itself using `npm run lint`.
`npm run lint`-ით შეგიძლიათ დარწმუნდეთ, რომ ეს მოდული საკუთარ თავს წარმატებით ამოწმებს.

[package-url]: https://npmjs.org/package/eslint-config-airbnb-base
[npm-version-svg]: https://versionbadg.es/airbnb/javascript.svg
[license-image]: https://img.shields.io/npm/l/eslint-config-airbnb-base.svg
[license-url]: LICENSE.md
[downloads-image]: https://img.shields.io/npm/dm/eslint-config-airbnb-base.svg
[downloads-url]: https://npm-stat.com/charts.html?package=eslint-config-airbnb-base
[actions-image]: https://img.shields.io/endpoint?url=https://github-actions-badge-u3jn4tfpocch.runkit.sh/airbnb/javascript
[actions-url]: https://github.com/airbnb/javascript/actions
