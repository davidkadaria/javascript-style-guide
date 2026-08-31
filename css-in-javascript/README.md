# Airbnb CSS-in-JavaScript Style Guide
# CSS-in-JavaScript-ის სტილისტიკის სახელმძღვანელო Airbnb-სგან

*A mostly reasonable approach to CSS-in-JavaScript*
*ყველაზე გონივრული მიდგომა CSS-in-JavaScript-ის საწერად*

## Table of Contents
## სარჩევი

1. [Naming](#naming)
1. [Ordering](#ordering)
1. [Nesting](#nesting)
1. [Inline](#inline)
1. [Themes](#themes)

## Naming
## სახელდება

  - Use camelCase for object keys (i.e. "selectors").
  - ობიექტის გასაღებებისთვის (ე.წ. „სელექტორებისთვის“) გამოიყენეთ camelCase.

    > Why? We access these keys as properties on the `styles` object in the component, so it is most convenient to use camelCase.
    > რატომ? ამ გასაღებებს კომპონენტში `styles` ობიექტის თვისებებად ვწვდებით, ამიტომ camelCase-ის გამოყენება ყველაზე მოსახერხებელია.

    ```js
    // ცუდია
    {
      'bermuda-triangle': {
        display: 'none',
      },
    }

    // კარგია
    {
      bermudaTriangle: {
        display: 'none',
      },
    }
    ```

  - Use an underscore for modifiers to other styles.
  - სხვა სტილების მოდიფიკატორებისთვის ქვეტირე გამოიყენეთ.

    > Why? Similar to [BEM](https://getbem.com/introduction/), this naming convention makes it clear that the styles are intended to modify the element preceded by the underscore. Underscores do not need to be quoted, so they are preferred over other characters, such as dashes.
    > რატომ? [BEM](https://getbem.com/introduction/)-ის მსგავსად, სახელდების ეს კანონზომიერება ცხადს ხდის, რომ სტილები იმ ელემენტის შესაცვლელადაა განკუთვნილი, რომელიც ქვეტირეს წინ უძღვის. ქვეტირეების ბრჭყალებში მოქცევა საჭირო არ არის, ამიტომ ისინი სხვა სიმბოლოებს, მაგალითად, ტირეებს სჯობს.

    ```js
    // ცუდია
    {
      bruceBanner: {
        color: 'pink',
        transition: 'color 10s',
      },

      bruceBannerTheHulk: {
        color: 'green',
      },
    }

    // კარგია
    {
      bruceBanner: {
        color: 'pink',
        transition: 'color 10s',
      },

      bruceBanner_theHulk: {
        color: 'green',
      },
    }
    ```

  - Use `selectorName_fallback` for sets of fallback styles.
  - სათადარიგო სტილების ნაკრებებისთვის გამოიყენეთ `selectorName_fallback`.

    > Why? Similar to modifiers, keeping the naming consistent helps reveal the relationship of these styles to the styles that override them in more adequate browsers.
    > რატომ? მოდიფიკატორების მსგავსად, სახელდების თანმიმდევრულობის შენარჩუნება ამ სტილების კავშირს წარმოაჩენს იმ სტილებთან, რომლებიც მათ უფრო შესაფერის ბრაუზერებში გადაფარავს.

    ```js
    // ცუდია
    {
      muscles: {
        display: 'flex',
      },

      muscles_sadBears: {
        width: '100%',
      },
    }

    // კარგია
    {
      muscles: {
        display: 'flex',
      },

      muscles_fallback: {
        width: '100%',
      },
    }
    ```

  - Use a separate selector for sets of fallback styles.
  - სათადარიგო სტილების ნაკრებებისთვის ცალკე სელექტორი გამოიყენეთ.

    > Why? Keeping fallback styles contained in a separate object clarifies their purpose, which improves readability.
    > რატომ? სათადარიგო სტილების ცალკე ობიექტში მოთავსება მათ დანიშნულებას ცხადს ხდის, რაც წაკითხვადობას აუმჯობესებს.

    ```js
    // ცუდია
    {
      muscles: {
        display: 'flex',
      },

      left: {
        flexGrow: 1,
        display: 'inline-block',
      },

      right: {
        display: 'inline-block',
      },
    }

    // კარგია
    {
      muscles: {
        display: 'flex',
      },

      left: {
        flexGrow: 1,
      },

      left_fallback: {
        display: 'inline-block',
      },

      right_fallback: {
        display: 'inline-block',
      },
    }
    ```

  - Use device-agnostic names (e.g. "small", "medium", and "large") to name media query breakpoints.
  - მედია-მოთხოვნების წყვეტის წერტილების სახელდებისთვის მოწყობილობისგან დამოუკიდებელი სახელები (მაგ.: „small“, „medium“ და „large“) გამოიყენეთ.

    > Why? Commonly used names like "phone", "tablet", and "desktop" do not match the characteristics of the devices in the real world. Using these names sets the wrong expectations.
    > რატომ? ხშირად გამოყენებული სახელები, როგორიცაა „phone“, „tablet“ და „desktop“, რეალურ სამყაროში არსებული მოწყობილობების მახასიათებლებს არ შეესაბამება. ამ სახელების გამოყენება არასწორ მოლოდინებს ქმნის.

    ```js
    // ცუდია
    const breakpoints = {
      mobile: '@media (max-width: 639px)',
      tablet: '@media (max-width: 1047px)',
      desktop: '@media (min-width: 1048px)',
    };

    // კარგია
    const breakpoints = {
      small: '@media (max-width: 639px)',
      medium: '@media (max-width: 1047px)',
      large: '@media (min-width: 1048px)',
    };
    ```

## Ordering
## თანმიმდევრობა

  - Define styles after the component.
  - სტილები კომპონენტის შემდეგ განსაზღვრეთ.

    > Why? We use a higher-order component to theme our styles, which is naturally used after the component definition. Passing the styles object directly to this function reduces indirection.
    > რატომ? ჩვენი სტილების თემატიზაციისთვის მაღალი რიგის კომპონენტს ვიყენებთ, რომელიც, ბუნებრივია, კომპონენტის განსაზღვრის შემდეგ გამოიყენება. სტილების ობიექტის უშუალოდ ამ ფუნქციისთვის გადაცემა არაპირდაპირობას ამცირებს.

    ```jsx
    // ცუდია
    const styles = {
      container: {
        display: 'inline-block',
      },
    };

    function MyComponent({ styles }) {
      return (
        <div {...css(styles.container)}>
          Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.
        </div>
      );
    }

    export default withStyles(() => styles)(MyComponent);

    // კარგია
    function MyComponent({ styles }) {
      return (
        <div {...css(styles.container)}>
          Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.
        </div>
      );
    }

    export default withStyles(() => ({
      container: {
        display: 'inline-block',
      },
    }))(MyComponent);
    ```

## Nesting
## ჩადგმა

  - Leave a blank line between adjacent blocks at the same indentation level.
  - აბზაცის ერთსა და იმავე დონეზე მდებარე მეზობელ ბლოკებს შორის ცარიელი ხაზი დატოვეთ.

    > Why? The whitespace improves readability and reduces the likelihood of merge conflicts.
    > რატომ? ინტერვალი წაკითხვადობას აუმჯობესებს და შერწყმის კონფლიქტების ალბათობას ამცირებს.

    ```js
    // ცუდია
    {
      bigBang: {
        display: 'inline-block',
        '::before': {
          content: "''",
        },
      },
      universe: {
        border: 'none',
      },
    }

    // კარგია
    {
      bigBang: {
        display: 'inline-block',

        '::before': {
          content: "''",
        },
      },

      universe: {
        border: 'none',
      },
    }
    ```

## Inline
## ხაზშიდა სტილები

  - Use inline styles for styles that have a high cardinality (e.g. uses the value of a prop) and not for styles that have a low cardinality.
  - ხაზშიდა (*inline*) სტილები გამოიყენეთ იმ სტილებისთვის, რომლებსაც მაღალი კარდინალობა აქვთ (მაგ.: prop-ის მნიშვნელობას იყენებს), და არა იმ სტილებისთვის, რომელთა კარდინალობა დაბალია.

    > Why? Generating themed stylesheets can be expensive, so they are best for discrete sets of styles.
    > რატომ? თემატიზებული სტილების ცხრილების გენერირება ძვირი შეიძლება იყოს, ამიტომ ისინი სტილების დისკრეტული ნაკრებებისთვისაა საუკეთესო.

    ```jsx
    // ცუდია
    export default function MyComponent({ spacing }) {
      return (
        <div style={{ display: 'table', margin: spacing }} />
      );
    }

    // კარგია
    function MyComponent({ styles, spacing }) {
      return (
        <div {...css(styles.periodic, { margin: spacing })} />
      );
    }
    export default withStyles(() => ({
      periodic: {
        display: 'table',
      },
    }))(MyComponent);
    ```

## Themes
## თემები

  - Use an abstraction layer such as [react-with-styles](https://github.com/airbnb/react-with-styles) that enables theming. *react-with-styles gives us things like `withStyles()`, `ThemedStyleSheet`, and `css()` which are used in some of the examples in this document.*
  - გამოიყენეთ აბსტრაქციის ფენა, როგორიც [react-with-styles](https://github.com/airbnb/react-with-styles) გახლავთ, რომელიც თემატიზაციის საშუალებას იძლევა. *react-with-styles გვაძლევს ისეთ რამეებს, როგორიცაა `withStyles()`, `ThemedStyleSheet` და `css()`, რომლებიც ამ დოკუმენტის ზოგიერთ მაგალითში გამოიყენება.*

  > Why? It is useful to have a set of shared variables for styling your components. Using an abstraction layer makes this more convenient. Additionally, this can help prevent your components from being tightly coupled to any particular underlying implementation, which gives you more freedom.
  > რატომ? სასარგებლოა, თქვენი კომპონენტების სტილიზაციისთვის საზიარო ცვლადების ნაკრები გქონდეთ. აბსტრაქციის ფენის გამოყენება ამას უფრო მოსახერხებელს ხდის. გარდა ამისა, ეს თქვენს კომპონენტებს რომელიმე კონკრეტულ ქვედა დონის რეალიზაციასთან მჭიდროდ დაკავშირებისგან იცავს, რაც მეტ თავისუფლებას გაძლევთ.

  - Define colors only in themes.
  - ფერები მხოლოდ თემებში განსაზღვრეთ.

    ```js
    // ცუდია
    export default withStyles(() => ({
      chuckNorris: {
        color: '#bada55',
      },
    }))(MyComponent);

    // კარგია
    export default withStyles(({ color }) => ({
      chuckNorris: {
        color: color.badass,
      },
    }))(MyComponent);
    ```

  - Define fonts only in themes.
  - შრიფტები მხოლოდ თემებში განსაზღვრეთ.

    ```js
    // ცუდია
    export default withStyles(() => ({
      towerOfPisa: {
        fontStyle: 'italic',
      },
    }))(MyComponent);

    // კარგია
    export default withStyles(({ font }) => ({
      towerOfPisa: {
        fontStyle: font.italic,
      },
    }))(MyComponent);
    ```

  - Define fonts as sets of related styles.
  - შრიფტები ურთიერთდაკავშირებული სტილების ნაკრებებად განსაზღვრეთ.

    ```js
    // ცუდია
    export default withStyles(() => ({
      towerOfPisa: {
        fontFamily: 'Italiana, "Times New Roman", serif',
        fontSize: '2em',
        fontStyle: 'italic',
        lineHeight: 1.5,
      },
    }))(MyComponent);

    // კარგია
    export default withStyles(({ font }) => ({
      towerOfPisa: {
        ...font.italian,
      },
    }))(MyComponent);
    ```

  - Define base grid units in theme (either as a value or a function that takes a multiplier).
  - ბადის საბაზისო ერთეულები თემაში განსაზღვრეთ (ან მნიშვნელობის სახით, ან ისეთი ფუნქციის სახით, რომელიც მამრავლს იღებს).

    ```js
    // ცუდია
    export default withStyles(() => ({
      rip: {
        bottom: '-6912px', // 6 feet
        // 6 ფუტი
      },
    }))(MyComponent);

    // კარგია
    export default withStyles(({ units }) => ({
      rip: {
        bottom: units(864), // 6 feet, assuming our unit is 8px
        // 6 ფუტი, თუკი ჩვენი ერთეული 8px-ია
      },
    }))(MyComponent);

    // კარგია
    export default withStyles(({ unit }) => ({
      rip: {
        bottom: 864 * unit, // 6 feet, assuming our unit is 8px
        // 6 ფუტი, თუკი ჩვენი ერთეული 8px-ია
      },
    }))(MyComponent);
    ```

  - Define media queries only in themes.
  - მედია-მოთხოვნები მხოლოდ თემებში განსაზღვრეთ.

    ```js
    // ცუდია
    export default withStyles(() => ({
      container: {
        width: '100%',

        '@media (max-width: 1047px)': {
          width: '50%',
        },
      },
    }))(MyComponent);

    // კარგია
    export default withStyles(({ breakpoint }) => ({
      container: {
        width: '100%',

        [breakpoint.medium]: {
          width: '50%',
        },
      },
    }))(MyComponent);
    ```

  - Define tricky fallback properties in themes.
  - რთული სათადარიგო თვისებები თემებში განსაზღვრეთ.

    > Why? Many CSS-in-JavaScript implementations merge style objects together which makes specifying fallbacks for the same property (e.g. `display`) a little tricky. To keep the approach unified, put these fallbacks in the theme.
    > რატომ? CSS-in-JavaScript-ის მრავალი რეალიზაცია სტილების ობიექტებს ერთმანეთს ურწყავს, რაც ერთი და იმავე თვისებისთვის (მაგ.: `display`) სათადარიგო მნიშვნელობების მითითებას ცოტა ართულებს. მიდგომის ერთიანობის შესანარჩუნებლად ეს სათადარიგო მნიშვნელობები თემაში მოათავსეთ.

    ```js
    // ცუდია
    export default withStyles(() => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        'display ': 'table',
      },
    }))(MyComponent);

    // კარგია
    export default withStyles(({ fallbacks }) => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        [fallbacks.display]: 'table',
      },
    }))(MyComponent);

    // კარგია
    export default withStyles(({ fallback }) => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        [fallback('display')]: 'table',
      },
    }))(MyComponent);
    ```

  - Create as few custom themes as possible. Many applications may only have one theme.
  - რაც შეიძლება ცოტა მომხმარებლური თემა შექმენით. ბევრ აპლიკაციას შესაძლოა მხოლოდ ერთი თემა ჰქონდეს.

  - Namespace custom theme settings under a nested object with a unique and descriptive key.
  - მომხმარებლური თემის პარამეტრები ჩადგმულ ობიექტში, უნიკალური და აღწერითი გასაღების ქვეშ მოათავსეთ (სახელთა სივრცე).

    ```js
    // ცუდია
    ThemedStyleSheet.registerTheme('mySection', {
      mySectionPrimaryColor: 'green',
    });

    // კარგია
    ThemedStyleSheet.registerTheme('mySection', {
      mySection: {
        primaryColor: 'green',
      },
    });
    ```

---

CSS puns adapted from [Saijo George](https://saijogeorge.com/css-puns/).
CSS-კალამბურები [Saijo George](https://saijogeorge.com/css-puns/)-ის მიხედვითაა ადაპტირებული.
