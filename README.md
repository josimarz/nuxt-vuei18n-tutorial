# How to use Vue-i18n with Nuxt.js

## Install packages

```sh
yarn add vue-i18n
# or
npm install vue-i18n --save
```

## Create a directory for translations

From source directory, create a new directory and name it as `locales`:

```sh
mkdir locales
```

## Create a localization file

As you should know, every country has its own regionalizations. For example, in Brazil the word "file" is translate as "arquivo", while in Portugal the same word is translated as "ficheiro". So, you can create a file named `pt-BR` for Brazi and a file named `pt-PT` for Portugal.

```js
// ~/locales/pt-BR.js

exports default {
  file: "Arquivo"
}
```

You must create a localization file for each combination of language and country to which you want to distribute a translation.

## Extending default translation

Vuetify provides a lot of translation pre-defined for their built-in components. You can extend that translation:

```js
// ~/locales/pt-BR.js

import { pt } from 'vuetify/es5/locale'

export default {
  $vuetify: pt,
  file: "Arquivo"
}
```

## Creating a plugin for i18n

To apply Vue-i18n to a Nuxt.js application, you must to create a plugin. You can name it as `i18n.js`:

```js
// ~/plugins/i18n.js

import Vue from 'vue'
import VueI18n from 'vue-i18n'

// import messages from localization files
import enUS from '~/locales/en-US'
import esES from '~/locales/es-ES'
import ptBR from '~/locales/pt-BR'

Vue.use(VueI18n)

export default ({ app, $vuetify }) => {
  // inject vue-i18n in the app context
  app.i18n = new VueI18n({
    locale: 'pt-BR',
    fallbackLocale: 'en-US',
    messages: {
      'en-US': enUS,
      'es-ES': esES,
      'pt-BR': ptBR,
    },
  })
  
  // change the default method used by Vuetify to apply translation to their built-in components
  $vuetify.lang.t = (key, ...params) => app.i18n.t(key, params)
}
```

## Using the plugin

First, you have to enable the plugin in the `nuxt.config.js` file:

```js
// ~/nuxt.config.js

// ...
/*
 ** Plugins to load before mounting the App
 ** https://nuxtjs.org/guide/plugins
 */
plugins: ['~/plugins/i18n.js'],
// ...
```

Then, from any `.vue` file, you can translate any key defined in the localization files:

```vue
<h5>{{ $t('file') }}</h5>
```
