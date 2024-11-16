<template>
  <div class="wrapper">
    <img src="bg.jpg" alt="Background">
    <header>
      <div id="header">
        <h1 style="background: url('./bg.jpg'); background-clip: text; -webkit-background-clip: text; -webkit-text-fill-color: transparent;">
          <!-- <img src="bg.jpg" alt="Background" class="header-bg"> -->
          Weather app
        </h1>
      </div>
      <DataTable />
    </header>
    <footer>
      <h3>(&ccaron;) {{ new Date().getFullYear() }} Vadim (<a href="http://1abcdesign.github.io">1abcdesign</a>) Chervoniak-Bogdanov</h3>
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
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}


h1::after {
  position: absolute;
  content: 'Weather app';
  color: transparent;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  top: 0;
  left: 50%;
  background: -webkit-linear-gradient(90deg, gold, navy);
  z-index: -1;
  transform: translateX(-50%);
}

body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100vh;
  color: rgb(240, 246, 252);
  overflow: hidden;
}

img {
  position: absolute;
  z-index: -1;
  height: 100vh;
}

@media (orientation: landscape) {
  img {
    object-fit: cover;
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
  img {
    object-fit: contain;
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
