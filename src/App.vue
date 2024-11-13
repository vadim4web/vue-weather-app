<template>
  <div class="wrapper">
    <header>
      <div id="header">
        <h1>Weather app</h1>
      </div>
      <DataTable />
    </header>
    <footer>
      <h3>(&ccaron;) {{ new Date().getFullYear() }} Bogdanov (<a href="http://8ogdanovv.github.io">8ogdanovv</a>) Vadim specially for FrontCo · Ukraine</h3>
    </footer>
  </div>
</template>

<script>

import DataTable from '@/components/DataTable.vue'

export default {
  name: 'App',
  components: {
    DataTable
  }
}
</script>

<style>
html {
  font-size: 3vh;
}

#header {
  background-color: white;
}

footer {
  width: 100%;
  background-color: rgb(128, 128, 128, 0.5);
}

h3 {
  display: inline-block;
  margin: 0;
  height: 100%;
  background: -webkit-linear-gradient(15deg, gold, navy);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: 900;
}

h1 {
  font-size: 3rem;
  background: -webkit-linear-gradient(315deg, rgb(253, 223, 38), #647eff);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-image: url('./assets/bg.jpg');
}

body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  background: rgb(22, 27, 34);
  background-image: url('./assets/bg.jpg');
  color: rgb(240, 246, 252);
}

@media (orientation: landscape) {
  body {
    background-size: cover;
  }

  footer {
    height: 2rem;
  }

  h3 {
    line-height: 2rem;
    font-size: 1.5rem;
  }
}

@media (orientation: portrait) {
  body {
    background-size: contain;
  }

  footer {
    height: 4rem;
  }

  h3 {
    line-height: 2rem;
    font-size: 1rem;
  }
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  text-align: center;
  height: 100vh;
  overflow: hidden;
}

.wrapper {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  text-align: center;
  height: 100%;
  margin: 1rem 0;
}
</style>
