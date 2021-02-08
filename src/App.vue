<template>
  <div id="app">
    <div class="container">
      <Controller
        ref="ctrl1"
        :extra="{
          text: 'Сумма',
          method: () => {
            this.$refs.ctrl1.number =
              +this.$refs.ctrl2.number + +this.$refs.ctrl3.number
          }
        }"
        :tab="() => ({ next: this.$refs.ctrl2, prev: this.$refs.ctrl3 })"
      />
      <Controller
        ref="ctrl2"
        :getItem="() => this.$refs.ctrl3"
        :extra="{
          text: 'Константа',
          method: () => (this.$refs.ctrl1.number = 1000)
        }"
        :tab="() => ({ next: this.$refs.ctrl3, prev: this.$refs.ctrl1 })"
      />
      <Controller
        ref="ctrl3"
        :getItem="() => this.$refs.ctrl2"
        :tab="() => ({ next: this.$refs.ctrl1, prev: this.$refs.ctrl2 })"
      />
    </div>
  </div>
</template>

<script>
import Controller from "./components/Controller.vue"

export default {
  name: "App",
  components: {
    Controller
  },
  data: () => ({}),
  methods: {},
  mounted() {}
}
</script>

<style lang="scss">
* {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #f5f5f5;
  background-color: #333;
}
.container {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
