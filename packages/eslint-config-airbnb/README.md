# eslint-config-airbnb

[![npm version](https://badge.fury.io/js/eslint-config-airbnb.svg)](https://badge.fury.io/js/eslint-config-airbnb)

This package provides Airbnb's .eslintrc as an extensible shared config.
ეს პაკეტი Airbnb-ის .eslintrc-ს გაფართოებადი საზიარო კონფიგურაციის სახით გთავაზობთ.

## Usage
## გამოყენება

We export three ESLint configurations for your usage.
თქვენთვის ESLint-ის სამ კონფიგურაციას ვაექსპორტებთ.

### eslint-config-airbnb

Our default export contains most of our ESLint rules, including ECMAScript 6+ and React. It requires `eslint`, `eslint-plugin-import`, `eslint-plugin-react`, `eslint-plugin-react-hooks`, and `eslint-plugin-jsx-a11y`. Note that it does not enable our React Hooks rules. To enable those, see the [`eslint-config-airbnb/hooks` section](#eslint-config-airbnbhooks).
ჩვენი ნაგულისხმევი ექსპორტი ESLint-ის ჩვენი წესების უმეტესობას შეიცავს, ECMAScript 6+-ისა და React-ის ჩათვლით. იგი მოითხოვს `eslint`-ს, `eslint-plugin-import`-ს, `eslint-plugin-react`-ს, `eslint-plugin-react-hooks`-სა და `eslint-plugin-jsx-a11y`-ს. გაითვალისწინეთ, რომ იგი React Hooks-ის ჩვენს წესებს არ რთავს. მათ ჩასართავად იხილეთ [`eslint-config-airbnb/hooks` განყოფილება](#eslint-config-airbnbhooks).

If you don't need React, see [eslint-config-airbnb-base](https://npmjs.com/eslint-config-airbnb-base).
თუკი React არ გჭირდებათ, იხილეთ [eslint-config-airbnb-base](https://npmjs.com/eslint-config-airbnb-base).

1. Install the correct versions of each package, which are listed by the command:
1. დააინსტალირეთ თითოეული პაკეტის სწორი ვერსია; მათ შემდეგი ბრძანება ჩამოთვლის:

  ```sh
  npm info "eslint-config-airbnb@latest" peerDependencies
  ```

  If using **npm 5+**, use this shortcut
  თუკი **npm 5+**-ს იყენებთ, გამოიყენეთ ეს მალსახმობი

  ```sh
  npx install-peerdeps --dev eslint-config-airbnb
  ```

  If using **yarn**, you can also use the shortcut described above if you have npm 5+ installed on your machine, as the command will detect that you are using yarn and will act accordingly.
  თუკი **yarn**-ს იყენებთ, ზემოთ აღწერილი მალსახმობის გამოყენებაც შეგიძლიათ, თუკი თქვენს მანქანაზე npm 5+ გაქვთ დაინსტალირებული, რადგანაც ბრძანება აღმოაჩენს, რომ yarn-ს იყენებთ, და შესაბამისად იმოქმედებს.
  Otherwise, run `npm info "eslint-config-airbnb@latest" peerDependencies` to list the peer dependencies and versions, then run `yarn add --dev <dependency>@<version>` for each listed peer dependency.
  წინააღმდეგ შემთხვევაში, თანმხლები დამოკიდებულებებისა (*peer dependencies*) და ვერსიების ჩამოსათვლელად გაუშვით `npm info "eslint-config-airbnb@latest" peerDependencies`, შემდეგ კი თითოეული ჩამოთვლილი თანმხლები დამოკიდებულებისთვის გაუშვით `yarn add --dev <dependency>@<version>`.

  If using **npm < 5**, Linux/OSX users can run
  თუკი **npm < 5**-ს იყენებთ, Linux/OSX-ის მომხმარებლებს შეუძლიათ გაუშვან

  ```sh
  (
    export PKG=eslint-config-airbnb;
    npm info "$PKG@latest" peerDependencies --json | command sed 's/[\{\},]//g ; s/: /@/g' | xargs npm install --save-dev "$PKG@latest"
  )
  ```

  Which produces and runs a command like:
  რაც შემდეგნაირ ბრძანებას ქმნის და უშვებს:

  ```sh
  npm install --save-dev eslint-config-airbnb eslint@^#.#.# eslint-plugin-jsx-a11y@^#.#.# eslint-plugin-import@^#.#.# eslint-plugin-react@^#.#.# eslint-plugin-react-hooks@^#.#.#
  ```

  If using **npm < 5**, Windows users can either install all the peer dependencies manually, or use the [install-peerdeps](https://github.com/nathanhleung/install-peerdeps) cli tool.
  თუკი **npm < 5**-ს იყენებთ, Windows-ის მომხმარებლებს შეუძლიათ ან ყველა თანმხლები დამოკიდებულება ხელით დააინსტალირონ, ან [install-peerdeps](https://github.com/nathanhleung/install-peerdeps) cli-ხელსაწყო გამოიყენონ.

  ```sh
  npm install -g install-peerdeps
  install-peerdeps --dev eslint-config-airbnb
  ```
  The cli will produce and run a command like:
  cli შემდეგნაირ ბრძანებას შექმნის და გაუშვებს:

  ```sh
  npm install --save-dev eslint-config-airbnb eslint@^#.#.# eslint-plugin-jsx-a11y@^#.#.# eslint-plugin-import@^#.#.# eslint-plugin-react@^#.#.# eslint-plugin-react-hooks@^#.#.#
  ```

2. Add `"extends": "airbnb"` to your `.eslintrc`
2. დაამატეთ `"extends": "airbnb"` თქვენს `.eslintrc`-ს

> **Note**: ESLint only lints `.js` files by default.
> **შენიშვნა**: ESLint ნაგულისხმევად მხოლოდ `.js` ფაილებს ამოწმებს.
  If your project uses `.jsx` (or `.tsx` with TypeScript), you need to pass extensions to the CLI:
  თუკი თქვენი პროექტი `.jsx`-ს (ან TypeScript-თან ერთად `.tsx`-ს) იყენებს, CLI-ს გაფართოებები უნდა გადასცეთ:

  ```sh
  eslint . --ext .js, .jsx, .mjs
  ```

  Without this, JSX-related rules will not apply to `.jsx` files.
  ამის გარეშე JSX-თან დაკავშირებული წესები `.jsx` ფაილებზე არ გავრცელდება.

### eslint-config-airbnb/hooks

This entry point enables the linting rules for React hooks (requires v16.8+). To use, add `"extends": ["airbnb", "airbnb/hooks"]` to your `.eslintrc`.
ეს შესასვლელი წერტილი React hooks-ისთვის შემოწმების წესებს რთავს (მოითხოვს v16.8+-ს). გამოსაყენებლად თქვენს `.eslintrc`-ს დაამატეთ `"extends": ["airbnb", "airbnb/hooks"]`.

### eslint-config-airbnb/whitespace

This entry point only errors on whitespace rules and sets all other rules to warnings. View the list of whitespace rules [here](https://github.com/airbnb/javascript/blob/master/packages/eslint-config-airbnb/whitespace.js).
ეს შესასვლელი წერტილი შეცდომებს მხოლოდ ინტერვალების წესებზე აგდებს, ყველა დანარჩენ წესს კი გაფრთხილებებად აყენებს. ინტერვალების წესების სია იხილეთ [აქ](https://github.com/airbnb/javascript/blob/master/packages/eslint-config-airbnb/whitespace.js).

### eslint-config-airbnb/base

This entry point is deprecated. See [eslint-config-airbnb-base](https://npmjs.com/eslint-config-airbnb-base).
ეს შესასვლელი წერტილი მოძველებულია. იხილეთ [eslint-config-airbnb-base](https://npmjs.com/eslint-config-airbnb-base).

### eslint-config-airbnb/legacy

This entry point is deprecated. See [eslint-config-airbnb-base](https://npmjs.com/eslint-config-airbnb-base).
ეს შესასვლელი წერტილი მოძველებულია. იხილეთ [eslint-config-airbnb-base](https://npmjs.com/eslint-config-airbnb-base).

See [Airbnb's JavaScript styleguide](https://github.com/airbnb/javascript) and
the [ESlint config docs](https://eslint.org/docs/user-guide/configuring#extending-configuration-files)
for more information.
დამატებითი ინფორმაციისათვის იხილეთ [Airbnb-ის JavaScript-ის სტილის სახელმძღვანელო](https://github.com/airbnb/javascript) და
[ESlint-ის კონფიგურაციის დოკუმენტაცია](https://eslint.org/docs/user-guide/configuring#extending-configuration-files).

## Improving this config
## ამ კონფიგურაციის გაუმჯობესება

Consider adding test cases if you're making complicated rules changes, like anything involving regexes. Perhaps in a distant future, we could use literate programming to structure our README as test cases for our .eslintrc?
თუკი წესებში რთულ ცვლილებებს შეგაქვთ, მაგალითად, რეგულარულ გამოსახულებებთან დაკავშირებულს, სატესტო შემთხვევების დამატება განიხილეთ. იქნებ შორეულ მომავალში ჩვენი README ჩვენივე .eslintrc-ის სატესტო შემთხვევებად „წიგნიერი პროგრამირების“ (*literate programming*) მეშვეობით დაგვესტრუქტურირებინა?

You can run tests with `npm test`.
ტესტების გაშვება `npm test`-ით შეგიძლიათ.

You can make sure this module lints with itself using `npm run lint`.
`npm run lint`-ით შეგიძლიათ დარწმუნდეთ, რომ ეს მოდული საკუთარ თავს წარმატებით ამოწმებს.
