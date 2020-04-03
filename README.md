# Vue  

## Terms  

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
   - **v-bind**: Binding syntax is as below. Bottom one is the shorthand.  
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
   - **Interpolation**: Pass the data using data() function on export.
   ``` 
   <div>Hey {{ name }}</div>
   ``` 

## Event Binding
   - use **v-on:event="method-name"** to bind events. bottom one is the shorthand.
   ``` 
      <button v-on:click="ok">OK</button>
      <button @click="ok">OK</button>
   ``` 
   

