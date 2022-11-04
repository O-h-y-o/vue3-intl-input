# vue3-intl-input

[added] binding mode 3

## [NPM](https://www.npmjs.com/package/vue3-intl-input) | [DOCS](https://vue3-intl-input-docs.netlify.app)

## Installation

use NPM -- is recommended

```sh{1}
$ npm install vue3-intl-input
```

use yarn

```sh
$ yarn add vue3-intl-input
```

use pnpm

```sh
$ pnpm install vue3-intl-input
```

## To use example

```js
// main.ts
import { createApp } from 'vue';
import App from './App.vue';
import vue3IntlInput from 'vue3-intl-input';
import 'vue3-intl-input/dist/style.css';

const app = createApp(App);
app.use(vue3IntlInput).mount('#app');
```

```js
// Component.vue
<template>
   <vue3-intl-input
    @sendInputValue="getInputValue"
    @sendCountryInfo="getCountryInfo"
    contentType="dropdown"
    noDataText="No Data Prop"
    title="INTERNATIONAL"
    placeholder="hi"
    searchPlaceholder="Hello"
    :effect="true"
    :clickCloseOutSide="true"
    :showTelInput="true"
    :showSearchCountryInput="true"
    :showDialCode="true"
    :bindingMode="1"
    :countriesList="['us', 'cn', 'kr', 'jp', 'ru', 'es', 'ph', 'th']"
  />
</template>

<script setup lang="ts">
const getInputValue = (val: object) => {
  console.log(val);
};

const getCountryInfo = (info: object) => {
  console.log(info);
};
</script>
```


