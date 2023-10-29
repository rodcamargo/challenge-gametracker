<template>
  <div id="app">

    <NavBar/>
    <p id="page-title">Ofertas</p>
    <Footer id="footer"/>

  </div>
</template>

<script>
import NavBar from './components/NavBar.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'App',
  components: {
    NavBar,
    Footer
  }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app {
  background: linear-gradient(to bottom left, #C70160, #0B1641);
  height: 100vh;
}

#page-title {
  font-family: 'Roboto', sans-serif;
  font-size: 36px;
  font-weight: 300;
  padding: 30px 0 9px 130px;
  color: #FFFFFF;
}

#footer {
  position: absolute;
  bottom: 0;
  width: 100%;
}

</style>
