<template>
  <div id="app">
    <pull-down @refresh="refresh">
      <li v-for="i in 30" @click="clickLi(i)">Item {{i}}</li>
    </pull-down>
  </div>
</template>

<script>
  import PullDown from './PullDown.vue';

  export default {
    name: 'app',
    components: {PullDown},
    data () {
      return {
      }
    },
    methods: {
      clickLi: function(i) {
        alert('Item ' + i + ' clicked.');
      },
      refresh: function() {
        console.log('onRefresh');
        // custom code
      }
    },
    mounted: function() {
    }
  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    background-color: #f4f4f4;
  }

  li {
    line-height: 30px;
  }
</style>
