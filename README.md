# Vue  

## Terms  
   - **Computed Properties**  
   These fire when any dependency value changes.  
   They are cached based on their dependencies.  
   Only reevaluates if reactive depndencies are changed.  
   ```
   computed: {
     fullName() {
       return `${this.selectedHero.firstName} ${this.selectedHero.lastName}`;
     },
   },
   ```
## Convention  
   - files in src/assets
   - components in src/components
   - design in src/design
   - views in src/views
   - name views with dash. **header-bar-brand.vue**

## What you need
   - install vue cli.
   - install **Vetur** and **Vue 2 Snippets** extensions for vscode

## Basic Vue Structure
```
<template>
  <div id="app">
    <HeaderBar />
    <div class="main-section content-title-group">
      <h2 class="title">Vue</h2>
    </div>
  </div>
</template>

<script>
import HeaderBar from '@/components/header-bar';

export default {
  name: 'App',
  components: { HeaderBar },
};
</script>

<style lang="scss">
@import '@/design/index.scss';
</style>
```
## Propety binding
   - **One way binding**: :property
   Binding syntax is as below. Bottom one is the shorthand.  
   ```
   <a v-bind:href="kerem">
   <a :href="kerem">
   ```  
   put the values using **data()** function. **data()** should return an object with the data to bind.
   ```  
   <script>
   export default {
     name: 'HeaderBarLinks',
     data() {
       return {
         kerem: 'https://github.com/kcakar',
       };
     },
   };
   </script>
   ```  
   - **Interpolation**: {{ }}  
   Pass the data using data() function on export.
   ``` 
   <div>Hey {{ name }}</div>
   ``` 
   - **Two way binding**: v-model
   ``` 
   <input class="input" id="lastName" v-model="hero.lastName" />
   ``` 
   - **Event binding**: @event
      - use **v-on:event="method-name"** to bind events. bottom one is the shorthand.
      ``` 
         <button v-on:click="ok">OK</button>
         <button @click="clickOk">OK</button>
         <div 
              class="color-line"
              :style="{ 'background-color': hero.capeColor }"
          ></div>
      ``` 
      - update models as below
      ```   
      <script>
      export default {
        name: 'Heroes',
        data() {
          return {
            hero: {
              id: 1,
              firstName: 'Kerem',
              lastName: 'Cakar',
              description: 'He drinks coffee',
              capeColor: '',
              power: '',
              active: true,
            },
            message: 'hey there',
          };
        },
        methods: {
          save() {
            this.message = JSON.stringify(this.hero, null, '\n');
          },
          cancel() {
            this.message = '';
          },
        },
      };
      </script>
      ``` 
      - Event binding to keys and classes
      ```
        <select
          id="power"
          v-model="hero.power"
          :class="{ invalid: !hero.power }"
          @keyup.esc="clearPower"
        >
          <option disabled value>Please select one</option>
          <option>Speed</option>
          <option>Invisibility</option>
        </select>
      ```
## Conditional content
   -  **v-for**: foreach loop. **Bind a unique :key for fast rendering**
   ```
     <ul class="list is-hoverable">
       <li v-for="hero in heroes" :key="hero.id" @click="selectHero(hero)" >
           <a class="list-item"
               :class="{'is-active':selectedHero==hero}"
           ><span>{{hero.firstName}} {{hero.lastName}}</span></a> 
       </li>
     </ul>
   ```
   -  **v-if**: Decides if the element should be rendered or not. If it is false, the element is removed from DOM.
   ```
   <div class="columns" v-if="isRender">
   ```
   -  **v-show**: The element is always on DOM. But hidden or shown depending on given value.
   
## Lifecycle Hooks
   - beforeCreate
   - created: fetch data here
   - beforeMounted
   - mounted
   DOM AVAILABLE
   - beforeUpdate
   - updated
   - beforeDestroy
   - destroyed
   
## Watchers  
   Use watchers to do something when a value on the model changes. They are good for async operations. The code below calls a function whenever a capeCounter value changes.
   ```
   watch: {
     'selectedHero.capeCounter': {
       immediate: true,
       handler(newValue, oldValue) {
         console.log(`Watcher evalauted. old=${oldValue}, new=${newValue}`);
         this.handleTheCapes(newValue);
       },
     },
   }
   ```
  
## FILTERS
   - Local filter:  
   ```
   <p class="comment">
      {{ selectedHero.originDate | shortDate }}
   </p>
   ```
   ```
   filters: {
    shortDate: function(value) {
      return format(value, displayDateFormat);
    },
  },
  ```
  - Global filter:  
  ```
  Vue.filter('capitalize', function (value) {
     if (!value) return '';
     value = value.toString();
     return value.charAt(0).toUpperCase() + value.slice(1);
  })

  new Vue({
     // ...
  })
  ```
  
  
  
  
  
  
  
  
  
  
  
