# Airbnb React/JSX Style Guide
# React/JSX-ის სტილისტიკის სახელმძღვანელო Airbnb-სგან

*A mostly reasonable approach to React and JSX*
*ყველაზე გონივრული მიდგომა React-სა და JSX-ზე კოდის საწერად*

This style guide is mostly based on the standards that are currently prevalent in JavaScript, although some conventions (i.e async/await or static class fields) may still be included or prohibited on a case-by-case basis. Currently, anything prior to stage 3 is not included nor recommended in this guide.
ეს სტილის სახელმძღვანელო, ძირითადად, იმ სტანდარტებს ეფუძნება, რომლებიც ამჟამად JavaScript-ში გაბატონებულია, თუმცა ზოგიერთი კანონზომიერება (მაგ.: async/await ან კლასის სტატიკური ველები) კვლავ შესაძლოა ცალკეულ შემთხვევებში იყოს ჩართული ან აკრძალული. ამჟამად ყველაფერი, რაც მე-3 ეტაპამდეა, ამ სახელმძღვანელოში არც შედის და არც რეკომენდებულია.

## Table of Contents
## სარჩევი

  1. [Basic Rules](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Mixins](#mixins)
  1. [Naming](#naming)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Refs](#refs)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [Ordering](#ordering)
  1. [`isMounted`](#ismounted)

## Basic Rules
## ძირითადი წესები

  - Only include one React component per file.
  - თითოეულ ფაილში მხოლოდ ერთი React კომპონენტი მოათავსეთ.
    - However, multiple [Stateless, or Pure, Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) are allowed per file. eslint: [`react/no-multi-comp`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
    - თუმცა ერთ ფაილში რამდენიმე [მდგომარეობის არმქონე, ანუ სუფთა, კომპონენტი](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions)[^1] დასაშვებია. eslint: [`react/no-multi-comp`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - Always use JSX syntax.
  - ყოველთვის გამოიყენეთ JSX სინტაქსი.
  - Do not use `React.createElement` unless you’re initializing the app from a file that is not JSX.
  - ნუ გამოიყენებთ `React.createElement`-ს, თუკი აპლიკაციის ინიციალიზაციას ისეთი ფაილიდან არ ახდენთ, რომელიც JSX არ არის.
  - [`react/forbid-prop-types`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/forbid-prop-types.md) will allow `arrays` and `objects` only if it is explicitly noted what `array` and `object` contains, using `arrayOf`, `objectOf`, or `shape`.
  - [`react/forbid-prop-types`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/forbid-prop-types.md) `arrays`-სა და `objects`-ს მხოლოდ მაშინ დაუშვებს, თუკი ცხადად არის მითითებული, თუ რას შეიცავს `array` და `object` — `arrayOf`-ის, `objectOf`-ის ან `shape`-ის მეშვეობით.

## Class vs `React.createClass` vs stateless
## კლასი vs `React.createClass` vs მდგომარეობის არმქონე

  - If you have internal state and/or refs, prefer `class extends React.Component` over `React.createClass`. eslint: [`react/prefer-es6-class`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)
  - თუკი შიდა მდგომარეობა (state) და/ან ref-ები გაქვთ, `React.createClass`-ს `class extends React.Component` ამჯობინეთ. eslint: [`react/prefer-es6-class`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

    ```jsx
    // ცუდია
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // კარგია
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    And if you don’t have state or refs, prefer normal functions (not arrow functions) over classes:
    ხოლო თუკი მდგომარეობა ან ref-ები არ გაქვთ, კლასებს ჩვეულებრივი ფუნქციები (და არა ისრისებური ფუნქციები) ამჯობინეთ:

    ```jsx
    // ცუდია
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // ცუდია (ფუნქციის სახელის ავტომატურ გამოცნობაზე დაყრდნობა რეკომენდებული არ არის)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // კარგია
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## Mixins
## მიქსინები

  - [Do not use mixins](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).
  - [ნუ გამოიყენებთ მიქსინებს](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html)[^2].

  > Why? Mixins introduce implicit dependencies, cause name clashes, and cause snowballing complexity. Most use cases for mixins can be accomplished in better ways via components, higher-order components, or utility modules.
  > რატომ? მიქსინებს არაცხადი დამოკიდებულებები შემოაქვს, სახელთა კონფლიქტებს იწვევს და სირთულის ზვავისებურ ზრდას განაპირობებს. მიქსინების გამოყენების უმეტესი შემთხვევა უკეთესი გზებით შეიძლება განხორციელდეს — კომპონენტების, მაღალი რიგის კომპონენტების[^3] ან დამხმარე მოდულების მეშვეობით.

## Naming
## სახელდება

  - **Extensions**: Use `.jsx` extension for React components. eslint: [`react/jsx-filename-extension`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-filename-extension.md)
  - **გაფართოებები**: React კომპონენტებისთვის გამოიყენეთ `.jsx` გაფართოება. eslint: [`react/jsx-filename-extension`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-filename-extension.md)
  - **Filename**: Use PascalCase for filenames. E.g., `ReservationCard.jsx`.
  - **ფაილის სახელი**: ფაილების სახელებისთვის გამოიყენეთ PascalCase. მაგ.: `ReservationCard.jsx`.
  - **Reference Naming**: Use PascalCase for React components and camelCase for their instances. eslint: [`react/jsx-pascal-case`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)
  - **მითითებების სახელდება**: React კომპონენტებისთვის გამოიყენეთ PascalCase, ხოლო მათი ინსტანციებისთვის — camelCase. eslint: [`react/jsx-pascal-case`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // ცუდია
    import reservationCard from './ReservationCard';

    // კარგია
    import ReservationCard from './ReservationCard';

    // ცუდია
    const ReservationItem = <ReservationCard />;

    // კარგია
    const reservationItem = <ReservationCard />;
    ```

  - **Component Naming**: Use the filename as the component name. For example, `ReservationCard.jsx` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.jsx` as the filename and use the directory name as the component name:
  - **კომპონენტების სახელდება**: კომპონენტის სახელად ფაილის სახელი გამოიყენეთ. მაგალითად, `ReservationCard.jsx`-ის მითითების სახელი `ReservationCard` უნდა იყოს. თუმცა დირექტორიის ძირეული კომპონენტებისთვის ფაილის სახელად `index.jsx` გამოიყენეთ, კომპონენტის სახელად კი — დირექტორიის სახელი:

    ```jsx
    // ცუდია
    import Footer from './Footer/Footer';

    // ცუდია
    import Footer from './Footer/index';

    // კარგია
    import Footer from './Footer';
    ```

  - **Higher-order Component Naming**: Use a composite of the higher-order component’s name and the passed-in component’s name as the `displayName` on the generated component. For example, the higher-order component `withFoo()`, when passed a component `Bar` should produce a component with a `displayName` of `withFoo(Bar)`.
  - **მაღალი რიგის კომპონენტების სახელდება**: გენერირებული კომპონენტის `displayName`-ად მაღალი რიგის კომპონენტისა და გადაცემული კომპონენტის სახელთა კომბინაცია გამოიყენეთ. მაგალითად, მაღალი რიგის კომპონენტმა `withFoo()`-მ, როდესაც მას კომპონენტი `Bar` გადაეცემა, ისეთი კომპონენტი უნდა შექმნას, რომლის `displayName` `withFoo(Bar)` იქნება.

    > Why? A component’s `displayName` may be used by developer tools or in error messages, and having a value that clearly expresses this relationship helps people understand what is happening.
    > რატომ? კომპონენტის `displayName` შესაძლოა დეველოპერულმა ხელსაწყოებმა ან შეცდომის შეტყობინებებმა გამოიყენოს, და ისეთი მნიშვნელობის ქონა, რომელიც ამ კავშირს ნათლად გამოხატავს, ადამიანებს იმის გაგებაში ეხმარება, თუ რა ხდება.

    ```jsx
    // ცუდია
    export default function withFoo(WrappedComponent) {
      return function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }
    }

    // კარგია
    export default function withFoo(WrappedComponent) {
      function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }

      const wrappedComponentName = WrappedComponent.displayName
        || WrappedComponent.name
        || 'Component';

      WithFoo.displayName = `withFoo(${wrappedComponentName})`;
      return WithFoo;
    }
    ```

  - **Props Naming**: Avoid using DOM component prop names for different purposes.
  - **Props-ების სახელდება**: მოერიდეთ DOM კომპონენტების prop-სახელების სხვა მიზნებისთვის გამოყენებას.

    > Why? People expect props like `style` and `className` to mean one specific thing. Varying this API for a subset of your app makes the code less readable and less maintainable, and may cause bugs.
    > რატომ? ადამიანები მოელიან, რომ ისეთი props-ები, როგორიც `style` და `className` გახლავთ, ერთ კონკრეტულ რამეს ნიშნავს. ამ API-ის შეცვლა თქვენი აპლიკაციის რომელიმე ნაწილისთვის კოდს ნაკლებად წაკითხვადსა და ნაკლებად მოვლადს ხდის და შესაძლოა ხარვეზები გამოიწვიოს.

    ```jsx
    // ცუდია
    <MyComponent style="fancy" />

    // ცუდია
    <MyComponent className="fancy" />

    // კარგია
    <MyComponent variant="fancy" />
    ```

## Declaration
## გამოცხადება

  - Do not use `displayName` for naming components. Instead, name the component by reference.
  - ნუ გამოიყენებთ `displayName`-ს კომპონენტების სახელდებისთვის. ამის ნაცვლად, კომპონენტს მითითებით დაარქვით სახელი.

    ```jsx
    // ცუდია
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
      // აქ შიგთავსია
    });

    // კარგია
    export default class ReservationCard extends React.Component {
    }
    ```

## Alignment
## განლაგება

  - Follow these alignment styles for JSX syntax. eslint: [`react/jsx-closing-bracket-location`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md) [`react/jsx-closing-tag-location`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)
  - JSX სინტაქსისთვის განლაგების ეს სტილები დაიცავით. eslint: [`react/jsx-closing-bracket-location`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md) [`react/jsx-closing-tag-location`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

    ```jsx
    // ცუდია
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // კარგია
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // if props fit in one line then keep it on the same line
    // თუკი props-ები ერთ ხაზზე ეტევა, იმავე ხაზზე დატოვეთ
    <Foo bar="bar" />

    // children get indented normally
    // შვილობილი ელემენტები ჩვეულებრივი აბზაცით იწერება
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>

    // ცუდია
    {showButton &&
      <Button />
    }

    // ცუდია
    {
      showButton &&
        <Button />
    }

    // კარგია
    {showButton && (
      <Button />
    )}

    // კარგია
    {showButton && <Button />}

    // კარგია
    {someReallyLongConditional
      && anotherLongConditional
      && (
        <Foo
          superLongParam="bar"
          anotherSuperLongParam="baz"
        />
      )
    }

    // კარგია
    {someConditional ? (
      <Foo />
    ) : (
      <Foo
        superLongParam="bar"
        anotherSuperLongParam="baz"
      />
    )}
    ```

## Quotes
## ბრჭყალები

  - Always use double quotes (`"`) for JSX attributes, but single quotes (`'`) for all other JS. eslint: [`jsx-quotes`](https://eslint.org/docs/rules/jsx-quotes)
  - JSX ატრიბუტებისთვის ყოველთვის ორმაგი ბრჭყალები (`"`) გამოიყენეთ, ხოლო დანარჩენი JS-ისთვის — ერთმაგი (`'`). eslint: [`jsx-quotes`](https://eslint.org/docs/rules/jsx-quotes)

    > Why? Regular HTML attributes also typically use double quotes instead of single, so JSX attributes mirror this convention.
    > რატომ? ჩვეულებრივი HTML ატრიბუტებიც, როგორც წესი, ერთმაგის ნაცვლად ორმაგ ბრჭყალებს იყენებს, ამიტომ JSX ატრიბუტები ამ კანონზომიერებას იმეორებს.

    ```jsx
    // ცუდია
    <Foo bar='bar' />

    // კარგია
    <Foo bar="bar" />

    // ცუდია
    <Foo style={{ left: "20px" }} />

    // კარგია
    <Foo style={{ left: '20px' }} />
    ```

## Spacing
## ინტერვალები

  - Always include a single space in your self-closing tag. eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)
  - თვითდამხურავ ტეგში ყოველთვის ერთი ინტერვალი ჩასვით. eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

    ```jsx
    // ცუდია
    <Foo/>

    // ძალიან ცუდია
    <Foo                 />

    // ცუდია
    <Foo
     />

    // კარგია
    <Foo />
    ```

  - Do not pad JSX curly braces with spaces. eslint: [`react/jsx-curly-spacing`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)
  - ნუ შეავსებთ JSX-ის ფიგურულ ფრჩხილებს ინტერვალებით. eslint: [`react/jsx-curly-spacing`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```jsx
    // ცუდია
    <Foo bar={ baz } />

    // კარგია
    <Foo bar={baz} />
    ```

## Props
## Props-ები

  - Always use camelCase for prop names, or PascalCase if the prop value is a React component.
  - prop-სახელებისთვის ყოველთვის camelCase გამოიყენეთ, ხოლო თუკი prop-ის მნიშვნელობა React კომპონენტია — PascalCase.

    ```jsx
    // ცუდია
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // კარგია
    <Foo
      userName="hello"
      phoneNumber={12345678}
      Component={SomeComponent}
    />
    ```

  - Omit the value of the prop when it is explicitly `true`. eslint: [`react/jsx-boolean-value`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)
  - გამოტოვეთ prop-ის მნიშვნელობა, როდესაც იგი ცხადად `true`-ა. eslint: [`react/jsx-boolean-value`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```jsx
    // ცუდია
    <Foo
      hidden={true}
    />

    // კარგია
    <Foo
      hidden
    />

    // კარგია
    <Foo hidden />
    ```

  - Always include an `alt` prop on `<img>` tags. If the image is presentational, `alt` can be an empty string or the `<img>` must have `role="presentation"`. eslint: [`jsx-a11y/alt-text`](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)
  - `<img>` ტეგებში ყოველთვის ჩართეთ `alt` prop-ი. თუკი გამოსახულება დეკორატიულია, `alt` შესაძლოა ცარიელი სტრიქონი იყოს, ან `<img>`-ს `role="presentation"` უნდა ჰქონდეს. eslint: [`jsx-a11y/alt-text`](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)

    ```jsx
    // ცუდია
    <img src="hello.jpg" />

    // კარგია
    <img src="hello.jpg" alt="Me waving hello" />

    // კარგია
    <img src="hello.jpg" alt="" />

    // კარგია
    <img src="hello.jpg" role="presentation" />
    ```

  - Do not use words like "image", "photo", or "picture" in `<img>` `alt` props. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)
  - ნუ გამოიყენებთ `<img>`-ის `alt` props-ებში ისეთ სიტყვებს, როგორიცაა „image“, „photo“ ან „picture“. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

    > Why? Screenreaders already announce `img` elements as images, so there is no need to include this information in the alt text.
    > რატომ? ეკრანის წამკითხველები[^4] `img` ელემენტებს ისედაც გამოსახულებებად აცხადებენ, ამიტომ ამ ინფორმაციის alt ტექსტში ჩართვა საჭირო არ არის.

    ```jsx
    // ცუდია
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // კარგია
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - Use only valid, non-abstract [ARIA roles](https://www.w3.org/TR/wai-aria/#usage). eslint: [`jsx-a11y/aria-role`](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)
  - გამოიყენეთ მხოლოდ ვალიდური, არააბსტრაქტული [ARIA როლები](https://www.w3.org/TR/wai-aria/#usage). eslint: [`jsx-a11y/aria-role`](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```jsx
    // ცუდია — ARIA როლი არ არის
    <div role="datepicker" />

    // ცუდია — აბსტრაქტული ARIA როლი
    <div role="range" />

    // კარგია
    <div role="button" />
    ```

  - Do not use `accessKey` on elements. eslint: [`jsx-a11y/no-access-key`](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)
  - ნუ გამოიყენებთ `accessKey`-ს ელემენტებზე. eslint: [`jsx-a11y/no-access-key`](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

  > Why? Inconsistencies between keyboard shortcuts and keyboard commands used by people using screenreaders and keyboards complicate accessibility.
  > რატომ? შეუსაბამობები კლავიატურის მალსახმობებსა და იმ კლავიატურულ ბრძანებებს შორის, რომლებსაც ეკრანის წამკითხველებისა და კლავიატურის მომხმარებლები იყენებენ, წვდომადობას ართულებს.

  ```jsx
  // ცუდია
  <div accessKey="h" />

  // კარგია
  <div />
  ```

  - Avoid using an array index as `key` prop, prefer a stable ID. eslint: [`react/no-array-index-key`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/no-array-index-key.md)
  - მოერიდეთ მასივის ინდექსის `key` prop-ად გამოყენებას, სტაბილური ID ამჯობინეთ. eslint: [`react/no-array-index-key`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/no-array-index-key.md)

> Why? Not using a stable ID [is an anti-pattern](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318) because it can negatively impact performance and cause issues with component state.
> რატომ? სტაბილური ID-ის გამოუყენებლობა [ცუდი პრაქტიკაა](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318), რადგანაც მან შესაძლოა წარმადობაზე უარყოფითად იმოქმედოს და კომპონენტის მდგომარეობასთან დაკავშირებული პრობლემები გამოიწვიოს.

We don’t recommend using indexes for keys if the order of items may change.
ჩვენ არ გირჩევთ ინდექსების გასაღებებად გამოყენებას, თუკი ელემენტების თანმიმდევრობა შესაძლოა შეიცვალოს.

  ```jsx
  // ცუდია
  {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
  )}

  // კარგია
  {todos.map(todo => (
    <Todo
      {...todo}
      key={todo.id}
    />
  ))}
  ```

  - Always define explicit defaultProps for all non-required props.
  - ყველა არასავალდებულო prop-ისთვის ყოველთვის ცხადი defaultProps განსაზღვრეთ.

  > Why? propTypes are a form of documentation, and providing defaultProps means the reader of your code doesn’t have to assume as much. In addition, it can mean that your code can omit certain type checks.
  > რატომ? propTypes დოკუმენტაციის ერთგვარი ფორმაა და defaultProps-ის მითითება ნიშნავს, რომ თქვენი კოდის მკითხველს ნაკლების ვარაუდი მოუწევს. გარდა ამისა, ეს შესაძლოა ნიშნავდეს, რომ თქვენს კოდს ტიპის ზოგიერთი შემოწმების გამოტოვება შეუძლია.

  ```jsx
  // ცუდია
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };

  // კარგია
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };
  SFC.defaultProps = {
    bar: '',
    children: null,
  };
  ```

  - Use spread props sparingly.
  - spread props-ები ზომიერად გამოიყენეთ.
  > Why? Otherwise you’re more likely to pass unnecessary props down to components. And for React v15.6.1 and older, you could [pass invalid HTML attributes to the DOM](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).
  > რატომ? წინააღმდეგ შემთხვევაში, უფრო სავარაუდოა, რომ კომპონენტებს ზედმეტ props-ებს გადასცემთ. ხოლო React v15.6.1-სა და უფრო ძველ ვერსიებში შესაძლოა [DOM-ისთვის არავალიდური HTML ატრიბუტები გადაგეცათ](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).

  Exceptions:
  გამონაკლისები:

  - HOCs that proxy down props and hoist propTypes
  - HOC-ები, რომლებიც props-ებს ქვემოთ გადასცემენ და propTypes-ს „სწევენ“

  ```jsx
  function HOC(WrappedComponent) {
    return class Proxy extends React.Component {
      Proxy.propTypes = {
        text: PropTypes.string,
        isLoading: PropTypes.bool
      };

      render() {
        return <WrappedComponent {...this.props} />
      }
    }
  }
  ```

  - Spreading objects with known, explicit props. This can be particularly useful when testing React components with Mocha’s beforeEach construct.
  - ცნობილი, ცხადი props-ების მქონე ობიექტების გაშლა. ეს განსაკუთრებით სასარგებლო შეიძლება იყოს React კომპონენტების ტესტირებისას Mocha-ს beforeEach კონსტრუქციით.

  ```jsx
  export default function Foo {
    const props = {
      text: '',
      isPublished: false
    }

    return (<div {...props} />);
  }
  ```

  Notes for use:
  შენიშვნები გამოყენებისთვის:
  Filter out unnecessary props when possible. Also, use [prop-types-exact](https://www.npmjs.com/package/prop-types-exact) to help prevent bugs.
  შეძლებისდაგვარად გაფილტრეთ ზედმეტი props-ები. ასევე, ხარვეზების თავიდან ასაცილებლად გამოიყენეთ [prop-types-exact](https://www.npmjs.com/package/prop-types-exact).

  ```jsx
  // ცუდია
  render() {
    const { irrelevantProp, ...relevantProps } = this.props;
    return <WrappedComponent {...this.props} />
  }

  // კარგია
  render() {
    const { irrelevantProp, ...relevantProps } = this.props;
    return <WrappedComponent {...relevantProps} />
  }
  ```

## Refs
## Ref-ები

  - Always use ref callbacks. eslint: [`react/no-string-refs`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)
  - ყოველთვის გამოიყენეთ ref callback-ები. eslint: [`react/no-string-refs`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

    ```jsx
    // ცუდია
    <Foo
      ref="myRef"
    />

    // კარგია
    <Foo
      ref={(ref) => { this.myRef = ref; }}
    />
    ```

## Parentheses
## მრგვალი ფრჩხილები

  - Wrap JSX tags in parentheses when they span more than one line. eslint: [`react/jsx-wrap-multilines`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)
  - მოაქციეთ JSX ტეგები მრგვალ ფრჩხილებში, როდესაც ისინი ერთზე მეტ ხაზს მოიცავს. eslint: [`react/jsx-wrap-multilines`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)

    ```jsx
    // ცუდია
    render() {
      return <MyComponent variant="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // კარგია
    render() {
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // კარგია, როდესაც ერთხაზიანია
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## Tags
## ტეგები

  - Always self-close tags that have no children. eslint: [`react/self-closing-comp`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)
  - ტეგები, რომლებსაც შვილობილი ელემენტები არ აქვთ, ყოველთვის თვითდახურეთ. eslint: [`react/self-closing-comp`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```jsx
    // ცუდია
    <Foo variant="stuff"></Foo>

    // კარგია
    <Foo variant="stuff" />
    ```

  - If your component has multiline properties, close its tag on a new line. eslint: [`react/jsx-closing-bracket-location`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)
  - თუკი თქვენს კომპონენტს მრავალხაზიანი თვისებები აქვს, მისი ტეგი ახალ ხაზზე დახურეთ. eslint: [`react/jsx-closing-bracket-location`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // ცუდია
    <Foo
      bar="bar"
      baz="baz" />

    // კარგია
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Methods
## მეთოდები

  - Use arrow functions to close over local variables. It is handy when you need to pass additional data to an event handler. Although, make sure they [do not massively hurt performance](https://www.bignerdranch.com/blog/choosing-the-best-approach-for-react-event-handlers/), in particular when passed to custom components that might be PureComponents, because they will trigger a possibly needless rerender every time.
  - ლოკალური ცვლადების ჩასაკეტად (*closure*) ისრისებური ფუნქციები გამოიყენეთ. ეს მოსახერხებელია, როდესაც მოვლენის დამმუშავებელს დამატებითი მონაცემების გადაცემა გჭირდებათ. თუმცა დარწმუნდით, რომ ისინი [წარმადობას მნიშვნელოვნად არ აზიანებს](https://www.bignerdranch.com/blog/choosing-the-best-approach-for-react-event-handlers/), განსაკუთრებით მაშინ, როდესაც ისეთ მომხმარებლურ კომპონენტებს გადაეცემა, რომლებიც შესაძლოა PureComponent-ები იყოს, რადგანაც ისინი ყოველ ჯერზე შესაძლოა ზედმეტ ხელახალ რენდერს გამოიწვევენ.

    ```jsx
    function ItemList(props) {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={(event) => { doSomethingWith(event, item.name, index); }}
            />
          ))}
        </ul>
      );
    }
    ```

  - Bind event handlers for the render method in the constructor. eslint: [`react/jsx-no-bind`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)
  - render მეთოდისთვის განკუთვნილი მოვლენის დამმუშავებლები კონსტრუქტორში მიაბით (bind). eslint: [`react/jsx-no-bind`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

    > Why? A bind call in the render path creates a brand new function on every single render. Do not use arrow functions in class fields, because it makes them [challenging to test and debug, and can negatively impact performance](https://medium.com/@charpeni/arrow-functions-in-class-properties-might-not-be-as-great-as-we-think-3b3551c440b1), and because conceptually, class fields are for data, not logic.
    > რატომ? render-ის გზაზე bind-ის გამოძახება ყოველი რენდერისას სრულიად ახალ ფუნქციას ქმნის. ნუ გამოიყენებთ ისრისებურ ფუნქციებს კლასის ველებში, რადგანაც ეს მათ [ტესტირებასა და გამართვას ართულებს და შესაძლოა წარმადობაზე უარყოფითად იმოქმედოს](https://medium.com/@charpeni/arrow-functions-in-class-properties-might-not-be-as-great-as-we-think-3b3551c440b1), და რადგანაც კონცეპტუალურად კლასის ველები მონაცემებისთვისაა და არა ლოგიკისთვის.

    ```jsx
    // ცუდია
    class extends React.Component {
      onClickDiv() {
        // do stuff
        // რაღაცას ვაკეთებთ
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />;
      }
    }

    // ძალიან ცუდია
    class extends React.Component {
      onClickDiv = () => {
        // do stuff
        // რაღაცას ვაკეთებთ
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }

    // კარგია
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
        // რაღაცას ვაკეთებთ
      }

      render() {
        return <div onClick={this.onClickDiv} />;
      }
    }
    ```

  - Do not use underscore prefix for internal methods of a React component.
  - ნუ გამოიყენებთ ქვეტირეს თავსართად React კომპონენტის შიდა მეთოდებისთვის.
    > Why? Underscore prefixes are sometimes used as a convention in other languages to denote privacy. But, unlike those languages, there is no native support for privacy in JavaScript, everything is public. Regardless of your intentions, adding underscore prefixes to your properties does not actually make them private, and any property (underscore-prefixed or not) should be treated as being public. See issues [#1024](https://github.com/airbnb/javascript/issues/1024), and [#490](https://github.com/airbnb/javascript/issues/490) for a more in-depth discussion.
    > რატომ? ქვეტირე-თავსართები სხვა ენებში ზოგჯერ პრივატულობის აღსანიშნავ კანონზომიერებად გამოიყენება. მაგრამ, იმ ენებისგან განსხვავებით, JavaScript-ს პრივატულობის ბუნებრივი მხარდაჭერა არ გააჩნია — ყველაფერი საჯაროა. თქვენი განზრახვის მიუხედავად, თქვენი თვისებებისთვის ქვეტირე-თავსართების დართვა მათ სინამდვილეში პრივატულს არ ხდის, და ნებისმიერი თვისება (ქვეტირე-თავსართით თუ მის გარეშე) საჯაროდ უნდა ჩაითვალოს. უფრო სიღრმისეული განხილვისთვის იხილეთ საკითხები [#1024](https://github.com/airbnb/javascript/issues/1024) და [#490](https://github.com/airbnb/javascript/issues/490).

    ```jsx
    // ცუდია
    React.createClass({
      _onClickSubmit() {
        // do stuff
        // რაღაცას ვაკეთებთ
      },

      // other stuff
      // სხვა შიგთავსი
    });

    // კარგია
    class extends React.Component {
      onClickSubmit() {
        // do stuff
        // რაღაცას ვაკეთებთ
      }

      // other stuff
      // სხვა შიგთავსი
    }
    ```

  - Be sure to return a value in your `render` methods. eslint: [`react/require-render-return`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/require-render-return.md)
  - დარწმუნდით, რომ თქვენს `render` მეთოდებში მნიშვნელობას აბრუნებთ. eslint: [`react/require-render-return`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/require-render-return.md)

    ```jsx
    // ცუდია
    render() {
      (<div />);
    }

    // კარგია
    render() {
      return (<div />);
    }
    ```

## Ordering
## თანმიმდევრობა

  - Ordering for `class extends React.Component`:
  - თანმიმდევრობა `class extends React.Component`-ისთვის:

  1. optional `static` methods
  1. არასავალდებულო `static` მეთოდები
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *event handlers starting with 'handle'* like `handleSubmit()` or `handleChangeDescription()`
  1. *მოვლენის დამმუშავებლები, რომლებიც 'handle'-ით იწყება*, მაგ.: `handleSubmit()` ან `handleChangeDescription()`
  1. *event handlers starting with 'on'* like `onClickSubmit()` or `onChangeDescription()`
  1. *მოვლენის დამმუშავებლები, რომლებიც 'on'-ით იწყება*, მაგ.: `onClickSubmit()` ან `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *getter მეთოდები `render`-ისთვის*, მაგ.: `getSelectReason()` ან `getFooterContent()`
  1. *optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. *არასავალდებულო render მეთოდები*, მაგ.: `renderNavigation()` ან `renderProfilePicture()`
  1. `render`

  - How to define `propTypes`, `defaultProps`, `contextTypes`, etc...
  - როგორ განვსაზღვროთ `propTypes`, `defaultProps`, `contextTypes` და ა.შ.

    ```jsx
    import React from 'react';
    import PropTypes from 'prop-types';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>;
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - Ordering for `React.createClass`: eslint: [`react/sort-comp`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)
  - თანმიმდევრობა `React.createClass`-ისთვის: eslint: [`react/sort-comp`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *clickHandler-ები ან eventHandler-ები*, მაგ.: `onClickSubmit()` ან `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *getter მეთოდები `render`-ისთვის*, მაგ.: `getSelectReason()` ან `getFooterContent()`
  1. *optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. *არასავალდებულო render მეთოდები*, მაგ.: `renderNavigation()` ან `renderProfilePicture()`
  1. `render`

## `isMounted`

  - Do not use `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)
  - ნუ გამოიყენებთ `isMounted`-ს. eslint: [`react/no-is-mounted`](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > Why? [`isMounted` is an anti-pattern][anti-pattern], is not available when using ES6 classes, and is on its way to being officially deprecated.
  > რატომ? [`isMounted` ცუდი პრაქტიკაა][anti-pattern], იგი ES6 კლასების გამოყენებისას ხელმისაწვდომი არ არის და ოფიციალურად მოძველებულად გამოცხადების გზაზეა.

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

## Translation
## თარგმანი

  This JSX/React style guide is also available in other languages:
  JSX/React-ის სტილის ეს სახელმძღვანელო სხვა ენებზეც არის ხელმისაწვდომი:

  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [jhcccc/javascript](https://github.com/jhcccc/javascript/tree/master/react)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript/tree/master/react)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Español**: [agrcrobles/javascript](https://github.com/agrcrobles/javascript/tree/master/react)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javascript-style-guide](https://github.com/mitsuruog/javascript-style-guide/tree/master/react)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [apple77y/javascript](https://github.com/apple77y/javascript/tree/master/react)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [pietraszekl/javascript](https://github.com/pietraszekl/javascript/tree/master/react)
  - ![Br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portuguese**: [ronal2do/javascript](https://github.com/ronal2do/airbnb-react-styleguide)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [leonidlebedev/javascript-airbnb](https://github.com/leonidlebedev/javascript-airbnb/tree/master/react)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide/tree/master/react)
  - ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [alioguzhan/react-style-guide](https://github.com/alioguzhan/react-style-guide)
  - ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [ivanzusko/javascript](https://github.com/ivanzusko/javascript/tree/master/react)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnam**: [uetcodecamp/jsx-style-guide](https://github.com/UETCodeCamp/jsx-style-guide)

**[⬆ ზემოთ](#table-of-contents)**

## სქოლიო

[^1]:
    კომპონენტი, რომელსაც შიდა მდგომარეობა (state) არ გააჩნია და მხოლოდ მიღებულ props-ებზეა დამოკიდებული (ინგლ.: Stateless component)
[^2]:
    კოდის ხელახალი გამოყენების მექანიზმი, რომლითაც კომპონენტს სხვა ობიექტის მეთოდები „ერევა“ (ინგლ.: Mixin)
[^3]:
    ფუნქცია, რომელიც კომპონენტს იღებს და ახალ კომპონენტს აბრუნებს (ინგლ.: Higher-order component, HOC)
[^4]:
    პროგრამა, რომელიც ეკრანის შიგთავსს ხმამაღლა კითხულობს უსინათლო და მცირემხედველი მომხმარებლებისთვის (ინგლ.: Screen reader)
