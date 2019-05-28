<template>
  <div class="home">
    <button @click="visible = !visible">选择时间！</button>
    <br>
    <div style="text-align: left; display: inline-block; margin: 10px auto;">
      <p>已选择的时间：</p>
      <p>时间：{{ time[0] || '无' }}</p>
      <p>时间戳：{{ time[1] || '无' }}</p>
    </div>
    <picker :visible.sync="visible" @confirm="onConfirm" :during="10" :interval="20"/>
  </div>
</template>

<script>
// @ is an alias to /src
import picker from "@/components/picker.vue";

export default {
  name: "home",
  components: {
    picker
  },
  data() {
    return {
      visible: false,
      time: []
    };
  },
  methods: {
    onConfirm(time, timestamp) {
      this.time = [].slice.apply(arguments);
      console.log(this.time);
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
