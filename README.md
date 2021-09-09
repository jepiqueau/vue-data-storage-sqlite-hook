<p align="center"><br><img src="https://avatars3.githubusercontent.com/u/16580653?v=4" width="128" height="128" /></p>

<h3 align="center">Vue Hook for capacitor-data-storage-sqlite plugin</h3>
<p align="center"><strong><code>vue-data-storage-sqlite-hook</code></strong></p>
<br>
<p align="center"><strong><code>Capacitor 3</code></strong></p>
<br>
<p align="center">
    <img src="https://img.shields.io/maintenance/yes/2021?style=flat-square" />
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

in the `App.vue` file

```ts
<template>
  <ion-app>
    <ion-router-outlet />
  </ion-app>
</template>

<script lang="ts">
import { IonApp, IonRouterOutlet } from '@ionic/vue';
import { defineComponent, getCurrentInstance, onMounted } from 'vue';
import { useStorageSQLite } from 'vue-data-storage-sqlite-hook/dist';

export default defineComponent({
  name: 'App',
  components: {
    IonApp,
    IonRouterOutlet
  },
  setup() {
    const app = getCurrentInstance();
    onMounted(async () => {
      if( app != null) {  
        app.appContext.config.globalProperties.$storageSqlite = useStorageSQLite();
      }
    });
  }
});
</script>
```

And then use the hook inside a component

```ts

<template>
  <div id="default-container">
    <div id="log">
        <pre>
          <p>{{log}}</p>
        </pre>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, getCurrentInstance, onMounted } from 'vue';
import { StorageSQLiteHook } from 'vue-data-storage-sqlite-hook/dist';
import { useState } from '@/composables/state';
import { Dialog } from '@capacitor/dialog';

export default defineComponent({
  name: 'DefaultTest',
  setup() {
    const [log, setLog] = useState("");
    const app = getCurrentInstance();
    const storageSqlite: StorageSQLiteHook = app?.appContext.config.globalProperties.$storageSqlite;
    let errMess = "";
    const showAlert = async (message: string) => {
        await Dialog.alert({
        title: 'Error Dialog',
        message: message,
        });
    };
    const defaultTest = async (): Promise<boolean>  => {
      setLog(log.value.concat("**** Starting Test Default Store ****\n"));
      try { 
        // open store
        await storageSqlite.openStore({});
        setLog(log.value.concat("openStore was successful \n"));
        // clear the store 
        await storageSqlite.clear();
        setLog(log.value.concat("clear was successful \n"));
        // store a string 
        await storageSqlite.setItem("session","Session Opened");
        const session = await storageSqlite.getItem('session');
        if(session != "Session Opened") {
          errMess = `setItem/getItem session failed`;
          return false;
        } else {
          setLog(log.value.concat("setItem/getItem session was successful \n"));
        }
        // store a JSON Object in the default store
        const data = {'a':20,'b':'Hello World','c':{'c1':40,'c2':'cool'}};
        await storageSqlite.setItem("testJson",JSON.stringify(data));
        const testJson = await storageSqlite.getItem("testJson");
        if( testJson != JSON.stringify(data) ) {
          errMess = `setItem/getItem testJson failed`;
          return false;
        } else {
          setLog(log.value
                .concat("setItem/getItem testJson was successful \n"));
        }
        // store a number in the default store
        const data1 = 243.567;
        await storageSqlite.setItem("testNumber",data1.toString());
        // read number from the store
        const testNumber = Number(await storageSqlite.getItem("testNumber"));
        if( testNumber != data1 ) {
          errMess = `setItem/getItem testNumber failed`;
          return false;
        } else {
          setLog(log.value
              .concat("setItem/getItem testNumber was successful \n"));
        }
        // isKey tests
        const iskey: boolean = await storageSqlite.isKey('testNumber');
        if( !iskey ) {
          errMess = `isKey testNumber failed`;
          return false;
        } else {
          setLog(log.value.concat("isKey testNumber was successful \n"));
        }
        const iskey1: boolean = await storageSqlite.isKey('foo');
        if( iskey1 ) {
          errMess = `isKey foo failed`;
          return false;
        } else {
          setLog(log.value.concat("isKey foo was successful \n"));
        }
        // Get All Keys
        const keys: string[] = await storageSqlite.getAllKeys();
        if(keys.length != 3) {
          errMess = `getAllKeys failed`;
          return false;
        } else {
          for(let i = 0; i< keys.length;i++) {
            setLog(log.value.concat(' key[' + i + "] = " + keys[i] + "\n"));
          }
          setLog(log.value.concat("getAllKeys was successful \n"));
        }
        // Get All Values
        const values: string[] = await storageSqlite.getAllValues();
        if(values.length != 3) {
          errMess = `getAllValues failed`;
          return false;
        } else {
          for(let i = 0; i< values.length;i++) {
            setLog(log.value.concat(' key[' + i + "] = " + values[i] + "\n"));
          }
          setLog(log.value.concat("getAllValues was successful \n"));
        }
        // Get All KeysValues
        const keysvalues: any[] = await storageSqlite.getAllKeysValues();
        if(keysvalues.length != 3) {
          errMess = `getAllKeysValues failed`;
          return false;
        } else {
          setLog(log.value.concat("getAllKeysValues was successful \n"));
        }
        // Remove a key 
        await storageSqlite.removeItem('testJson')
        setLog(log.value.concat("removeItem was successful \n"));

        // Get All Keys
        const keys1: string[] = await storageSqlite.getAllKeys();
        if(keys1.length != 2) {
          errMess = `getAllKeys after removeItem failed`;
          return false;
        } else {
          for(let i = 0; i< keys1.length;i++) {
            setLog(log.value.concat(' key[' + i + "] = " + keys1[i] + "\n"));
          }
          setLog(log.value
                .concat("getAllKeys after removeItem was successful \n"));
        }
        setLog(log.value
          .concat("\n**** Test Default Store was successful ****\n")); 
        return true;
      } catch (err) {
          errMess = `${err.message}`;
          return false;
      }
    };
    onMounted(async () => {
        // Running the test
        const retDefaultTest: boolean = await defaultTest();
        console.log(`retDefaultTest ${retDefaultTest}`);
        if(!retDefaultTest) {
            setLog(log.value
                .concat("* defaultTest failed *\n"));
            setLog(log.value
                    .concat("\n* The set of tests failed *\n"));
            await showAlert(errMess);
        } else {
            setLog(log.value
                .concat("\n* The set of tests was successful *\n"));
        }

    });

    return { log, errMess };
  }

});
</script>

```



## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://github.com/jepiqueau"><img src="https://avatars3.githubusercontent.com/u/16580653?v=4" width="100px;" alt=""/><br /><sub><b>Jean Pierre Quéau</b></sub></a><br /><a href="https://github.com/jepiqueau/vue-data-storage-sqlite-hook/commits?author=jepiqueau" title="Code">💻</a></td>
  </tr>
</table>

<!-- markdownlint-enable -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!

