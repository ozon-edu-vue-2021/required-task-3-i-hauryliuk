<template>
    <div id="app">
        <Office />
    </div>
</template>

<script>
import Office from './components/Office.vue';

export default {
  name: "App",
  components: {
    Office,
  },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    color: #2c3e50;
    background-color: #fafafa;
    padding: 24px;
    box-sizing: border-box;
}

html,
body,
#app {
    height: 100%;
}

* {
    box-sizing: border-box;
}

h3 {
    margin-top: 0px;
}
</style>
