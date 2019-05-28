<template>
  <div class="home">
    <button @click="visible = !visible">选择时间！</button>

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
      visible: false
    };
  },
  methods: {
    onConfirm(time, timestamp) {
      console.log([].slice.apply(arguments));
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
