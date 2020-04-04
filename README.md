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
   - **Kebab case**: this-is-kebab-case
   - **Mixins**: These are the common functionality parts of code. Like helpers. They can have methods, computeds, life cycle hooks, data, watches etc. If you have conflict with the component, then the component always have precedence and the rest is merged. Except for watch and hooks. Mixing watch and hooks run before the components run.  
   **Component**:  
   ```
   mixins: [lifecycleHooks, heroWatchers],
   ```  
   **Mixin**:
   ```
   export const heroWatchers = {
     // Watchers
     watch: {
       selectedHero: {
         immediate: true,
         deep: true,
         handler(newValue, oldValue) {
           logger.info('old values', oldValue);
         },
       },
     },
   };
   ```
## Convention  
   - files in src/assets
   - any component that you route to is in src/views
   - components in src/components
   - design in src/design
   - views in src/views
   - mixins in src/shared
   - name views with dash. **header-bar-brand.vue**
   - name variables with camelCase. If you pass props to a child, use kebab-case in the child.

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
## Component registration
   - Global registration
   ```
   Vue.component('my-component', MyComponent);
   ```
   - Local registration  
   ```
   import MyComponent from './MyComponent.vue';

   export default {
     name: 'AnotherComponent',
     components: {
       MyComponent,
     }
   };
   ```
## Passing variables across components  
   - **Props**:
     - Props can have types.
     - Props can be marked as required.
     - Props can have custom validator.  
     - **ALWAYS** clone the props at the child if you are going to modify them. Then raise an event when you would want to save.  
   **Parent**:  
   ```
   <HeroDetail v-if="selectedHero" :hero="selectedHero" />
   ```  
   **Child**:  
   ```
   <header class="card-header">
     <p class="card-header-title">{{ detailedHero.firstName }}</p>
   </header>
      
   <script>
   export default {
     name: 'HeroDetail',
     data: (){
       detailedHero: {...this.hero},
     },
     props: {
       hero: {
         type: Object,
         default: () => {},
       },
     },
   };
   </script>
   ```
   - **Events**  
   **Child**:  
   ```
   methods: {
    cancelHero() {
      this.$emit('cancel');
    },
    saveHero() {
      this.$emit('save', this.clonedHero);
    },
   }
   ```  
   **Parent**:  
   ```
   <HeroDetail
      :hero="selectedHero"
      @save="saveHero"
      @cancel="cancelHero"
      v-if="selectedHero"
   />
   
   methods: 
     cancelHero() {
       this.selectedHero = undefined;
     },
     saveHero(hero) {
       const index = this.heroes.findIndex(h => h.id === hero.id);
       this.heroes.splice(index, 1, hero);
       this.heroes = [...this.heroes];
       this.selectedHero = undefined;
     }
   },
   ```
## Axios
   - has GET,POST,PUT,DELETE methods.
   - returns a promise
   - try to use async/await syntax
   
## Vue Router  
   vue add router  
   - You can lazy load components. **everything that is not required on initial render** should be lazy loaded. **You should always use v-if** instead of v-show for lazy loaded components. **ALSO PASS THE COMPONENT AS A FUNCTION**. You should also bundle similar things together to save traffic.
   ```
   <template>
     <div>
       <button @click="isModalVisible = true">Open modal</button>
       <modal-window v-if="isModalVisible" />
     </div>
   </template>

   <script>
   export default {
     data () {
       return {
         isModalVisible: false
       }
     },
     components: {
       ModalWindow: () => import(/* webpackPrefetch: true */ './ModalWindow.vue')
     }
   }
   </script>
   ```
   - add router element to the app.vue
   ```
   <router-view></router-view>
   ```
   - add router links to redirect. This link automatically adds "router-link-exact-active router-link-active" classes so it is possible to style them with these classes.
   ```
   <router-link to="/about"></router-link>
   <router-link 
      :to="{ name: 'hero-detail', params: { id: hero.id }}"
      tag="button"
      class="link"
   ></router-link>
   ```
   - router.js
   ```
   import Vue from 'vue';
   import Router from 'vue-router';
   import Heroes from './views/heroes.vue';

   Vue.use(Router);
   
   const parseProps = r => ({ id: parseInt(r.params.id) })

   export default new Router({
     mode: 'history',
     base: process.env.BASE_URL,
     routes: [
       {
         path: '/',
         redirect: '/heroes',
       },
       {
         path: '/heroes',
         name: 'heroes',
         component: Heroes,
       },
       {
         path: '/villains',
         name: 'villains',
         component: Heroes,
       },
       {
         path: '/heroes/:id',
         name: 'hero-detail',
         component: HeroDetail,
         props: parseProps,
       },
       //LAZY LOADING VERSION OF THE ABOVE
       {
         path: '/heroes/:id',
         name: 'hero-detail',
         // props: true,
         props: parseProps,
         component: () =>
           import(/* webpackChunkName: "bundle.heroes" */ './views/hero-detail.vue'),
       }
     ],
   });
   ```
   - programmatical routing
   ```
   methods: {
    cancelHero() {
      this.$router.push({ name: 'heroes' });
    },
    async saveHero() {
      await dataService.updateHero(this.hero);
      this.$router.push({ name: 'heroes' });
    },
  }
  ```
  
  
  
  
