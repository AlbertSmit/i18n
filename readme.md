# i18n

Internationalize your React application.

## Motivation
i18n a.k.a. internationalization. More often than not you want to make your application available in multiple languages. @kaliber/i18n helps translating the static strings in your application.

## Polyfill
This module uses a Symbol to check wether a `I18nContextProvider` is available. At the time of writing, `Symbol` is [supported by most modern browsers](https://caniuse.com/#feat=mdn-javascript_builtins_symbol), but if you need to support IE11 you will have to polyfill it (Symbol is included in polyfill.io's `es2015` polyfill). 

## Installation

```
yarn add @kaliber/i18n
```

## Usage

[Skip to reference](#reference)

```jsx
import { I18nContextProvider, I18nSection, useI18n, useI18nLanguage } from '@kaliber/i18n'
import { i18n } from './config/i18n.js'

function App() {
  return (
    <I18nContextProvider value={i18n} language='en'>
      <Page />
    </I18nContextProvider>
  )
}

function Page() {
  const i18n = useI18n()
  const language = useI18nLanguage()

  return (
    <div>
      <header>
        {i18n.navigation.map(({ label, link }, i) => (
          <a key={i} href={link}>{label}</a>
        ))}
      </header>
      <main>
        <I18nSection section='home'>
          <Home />
        </I18nSection>
      </main>
      <footer>
        {i18n.footer.copyright(2020)} - {language.toUpperCase()}
      </footer>
    </div>
  )
}

function Home() {
  const i18n = useI18n()

  return (
    <article>
      <h1>{i18n.title}</h1>
      <Intro />
    </article>
  )
}

function Intro() {
  const i18n = useI18n('intro')

  return (
    <section>
      <h2>
        {i18n.title}
      </h2>
      <div>
        {i18n.body}
      </div>
    </section>
  )
}
```

Example i18n object:
```js
const i18n = {
  navigation: [
    {
      label: 'Home',
      link: '#'
    },
    {
      label: {
        nl: 'Neem contact op',
        en: 'Contact us'
      },
      link: '#contact'
    }
  ],
  home: {
    title: {
      nl: 'Welkom!',
      en: 'Welcome!'
    },
    intro: {
      title: {
        nl: 'Een klein beetje typesetting tekst',
        en: 'A bit of typesetting text'
      },
      body: <p>Lorem ipsum dolor sit amet <strong>consectetur adipisicing elit</strong>.</p>,
    },
  },
  footer: {
    copyright: year => `Copyright ${year}`
  }
}
```

![](https://media.giphy.com/media/OBP5KeYczcxxK/giphy.gif)

## Reference

### `I18nContextProvider`

```jsx
<Provider value={i18n} language='en'>
  {children}
</Provider>
```

| Props          |                                                                               |
|----------------|-------------------------------------------------------------------------------|
| `value`        | The i18n object to use. |
| `language`     | _Optional._ <br />When a language is given, normalization will be attempted at the leave nodes. E.g.: given the language `en` this means a leafnode with the shape `{ en: 'countries', nl: 'landen' }` will be normalized to just `'countries'`. |

The provided i18n object may be a deeply nested object, optionally containing arrays. Values don't have to be strings, you can also provide numbers, functions or React elements (see the example i18n object under [usage](#usage))

### `I18nSection`

```jsx
<I18nSection section='home'>
  {children}
</I18nSection>
```

| Props          |                                                                               |
|----------------|-------------------------------------------------------------------------------|
| `section`      | Re-provides a subsection of the i18n object indicated by `section`. `section` should be a string corresponding to a property in the current i18n value. Dots may be used: `'home.introduction'` is a valid value. |

### `useI18n`

Returns (a subsection of) the i18n object from the nearest provider.

```js
const i18n = useI18n(section) 
```

| Input          |                                                                               |
|----------------|-------------------------------------------------------------------------------|
| `section`      | _Optional._ <br />When given, a subsection of the i18n object indicated by `section` will be returned. `section` should be a string corresponding to a property in the current i18n value. Dots may be used: `'home.introduction'` is a valid value. |

### `useI18nLanguage`

Returns the language that is being used.

```js
const language = useI18nLanguage() 
```

## Disclaimer
This library is intended for internal use, we provide __no__ support, use at your own risk. It does not import React, but expects it to be provided, which [@kaliber/build](https://kaliberjs.github.io/build/) can handle for you.

This library is not transpiled.