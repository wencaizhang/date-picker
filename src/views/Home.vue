<template>
  <div class="home">
    <button @click="visible = !visible">选择时间！</button>
    
    <picker :visible.sync="visible" @confirm="onConfirm" :during="10" startTime="2019-05-21 11:20:22"/>
  </div>
</template>

<script>
// @ is an alias to /src
import picker from '@/components/picker.vue';

export default {
  name: 'home',
  components: {
    picker,
  },
  data () {
    return {
      visible: false,
    }
  },
  methods: {
    onConfirm (time, timestamp) {
      console.log( [].slice.apply(arguments))
    }
  }
};
</script>
<style>
button {
  margin-top: 30px;
  padding: 40px 20px;
}
</style>
