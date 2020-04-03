# Vue  

## Terms  

## Convention  

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
   
   


