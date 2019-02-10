<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>

import Vue from 'vue';

import 'es6-promise/auto'; // required for Vuex old browsers compatibility
import Vuex from 'vuex'
Vue.use(Vuex)

export default {
  name: 'App'
}
</script>
