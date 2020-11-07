<p align="center"><br><img src="https://avatars3.githubusercontent.com/u/16580653?v=4" width="128" height="128" /></p>

<h3 align="center">Vue Hook for capacitor-data-storage-sqlite plugin</h3>
<p align="center"><strong><code>vue-data-storage-sqlite-hook</code></strong></p>
<br>
<p align="center">
    <img src="https://img.shields.io/maintenance/yes/2020?style=flat-square" />
    <a href="https://www.npmjs.com/package/vue-data-storage-sqlite-hook"><img src="https://img.shields.io/npm/l/vue-data-storage-sqlite-hook?style=flat-square" /></a>
<br>
  <a href="https://www.npmjs.com/package/vue-data-storage-sqlite-hook"><img src="https://img.shields.io/npm/dw/vue-data-storage-sqlite-hook?style=flat-square" /></a>
  <a href="https://www.npmjs.com/package/vue-data-storage-sqlite-hook"><img src="https://img.shields.io/npm/v/vue-data-storage-sqlite-hook?style=flat-square" /></a>
<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
<a href="#contributors-"><img src="https://img.shields.io/badge/all%20contributors-1-orange?style=flat-square" /></a>
<!-- ALL-CONTRIBUTORS-BADGE:END -->
</p>

## Maintainers

| Maintainer        | GitHub                                    | Social |
| ----------------- | ----------------------------------------- | ------ |
| Quéau Jean Pierre | [jepiqueau](https://github.com/jepiqueau) |        |



## Installation

```bash
npm install --save capacitor-data-storage-sqlite
npm install --save-dev vue-data-storage-sqlite-hook
```

## Applications demonstrating the use of the plugin and the vue hook

### Vue 3
 - [vue-datastoragesqlite-app](https://github.com/jepiqueau/vue-datastoragesqlite-app)

### Ionic/Vue
 - [vue-data-storage-sqlite-app-starter](https://github.com/jepiqueau/vue-data-storage-sqlite-app-starter)


## Usage
Import the hook from its own path:

```js
 import { useStorageSQLite } from 'vue-data-storage-sqlite-hook/dist';
```

Then use the hook inside a component

```js

<template>
  <div id="default-container">
    <div id="log">
        <pre>
          <p>{{log}}</p>
        </pre>
    </div>
  </div>
</template>

<script >
import { defineComponent, ref } from 'vue';
import { useStorageSQLite } from 'vue-data-storage-sqlite-hook/dist';

export default defineComponent({
  name: 'DefaultTest',
  async setup() {
    const log = ref("");
    const { openStore, getItem, setItem, getAllKeys, getAllValues,
            getAllKeysValues, isKey, removeItem,
            clear} = useStorageSQLite();
    log.value = log.value.concat("**** Starting Test Default Store ****\n"); 
    // open store
    const resOpen = await openStore({});
    if( !resOpen ) {
      log.value = log.value.concat("openStore failed \n");
      return { log };
    } else {
      log.value = log.value.concat("openStore was successful \n");
    }
    // clear the store 
    const rClear = await clear();
    if( !rClear ) {
      log.value = log.value.concat("clear failed \n");
      return { log };
    } else {
      log.value = log.value.concat("clear was successful \n");
    }
    // store a string 
    await setItem("session","Session Opened");
    const session = await getItem('session');
    if(session != "Session Opened") {
      log.value = log.value.concat("setItem/getItem session failed \n");
      return { log };
    } else {
      log.value = log.value.concat("setItem/getItem session was successful \n");
    }
    // store a JSON Object in the default store
    const data = {'a':20,'b':'Hello World','c':{'c1':40,'c2':'cool'}};
    await setItem("testJson",JSON.stringify(data));
    const testJson = await getItem("testJson");
    if( testJson != JSON.stringify(data) ) {
      log.value = log.value.concat("setItem/getItem testJson failed \n");
      return { log };
    } else {
      log.value = log.value.concat("setItem/getItem testJson was successful \n");
    }
    // store a number in the default store
    const data1 = 243.567;
    await setItem("testNumber",data1.toString());
    // read number from the store
    const testNumber = await getItem("testNumber");
    if( testNumber != data1 ) {
      log.value = log.value.concat("setItem/getItem testNumber failed \n");
      return { log };
    } else {
      log.value = log.value.concat("setItem/getItem testNumber was successful \n");
    }
    // isKey tests
    const iskey = await isKey('testNumber');
    if( !iskey ) {
      log.value = log.value.concat("isKey testNumber failed \n");
      return { log };
    } else {
      log.value = log.value.concat("isKey testNumber was successful \n");
    }
    const iskey1 = await isKey('foo');
    if( iskey1 ) {
      log.value = log.value.concat("isKey foo failed \n");
      return { log };
    } else {
      log.value = log.value.concat("isKey foo was successful \n");
    }
    // Get All Keys
    const keys = await getAllKeys();
    if(keys.length != 3) {
      log.value = log.value.concat("getAllKeys failed \n");
      return { log };
    } else {
      for(let i = 0; i< keys.length;i++) {
        log.value = log.value.concat(' key[' + i + "] = " + keys[i] + "\n");
      }
      log.value = log.value.concat("getAllKeys was successful \n");
    }
    // Get All Values
    const values = await getAllValues();
    if(values.length != 3) {
      log.value = log.value.concat("getAllValues failed \n");
      return { log };
    } else {
      for(let i = 0; i< values.length;i++) {
        log.value = log.value.concat(' key[' + i + "] = " + values[i] + "\n");
      }
      log.value = log.value.concat("getAllValues was successful \n");
    }
    // Get All KeysValues
    const keysvalues = await getAllKeysValues();
    if(keysvalues.length != 3) {
      log.value = log.value.concat("getAllKeysValues failed \n");
      return { log };
    } else {
      log.value = log.value.concat("getAllKeysValues was successful \n");
    }
    // Remove a key 
    const res = await removeItem('testJson')
    if( !res ) {
      log.value = log.value.concat("removeItem failed \n");
      return { log };
    } else {
      log.value = log.value.concat("removeItem was successful \n");
    }
    // Get All Keys
    const keys1 = await getAllKeys();
    if(keys1.length != 2) {
      log.value = log.value.concat("getAllKeys after removeItem failed \n");
      return { log };
    } else {
      for(let i = 0; i< keys1.length;i++) {
        log.value = log.value.concat(' key[' + i + "] = " + keys1[i] + "\n");
      }
      log.value = log.value.concat("getAllKeys after removeItem was successful \n");
    }
    log.value = log.value.concat("\n**** Test Default Store was successful ****\n"); 
    return { log };
  }

});
</script>  
```

and then call the component from a view

```html
<template>
  <Suspense>
    <template #default>
      <DefaultTest />
    </template>
    <template #feedback>
      <div> Loading ... </div>
    </template>
  </Suspense>
</template>
<script>
import DefaultTest from '@/components/DefaultTest.vue';
export default {
  name: 'StoreDefault',
  components: { DefaultTest }
};
</script>
<style>
h1 {
    text-align: center;
}
</style>
```



## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://github.com/jepiqueau"><img src="https://avatars3.githubusercontent.com/u/16580653?v=4" width="100px;" alt=""/><br /><sub><b>Jean Pierre Quéau</b></sub></a><br /><a href="https://github.com/jepiqueau/vue-data-storage-sqlite-app-starter/commits?author=jepiqueau" title="Code">💻</a></td>
  </tr>
</table>

<!-- markdownlint-enable -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!

