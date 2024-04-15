<template>
  <Navbar :logo="logo_src" :alt="app_name"/>
  
  <router-view />

  

  <Footer />
</template>

<script>
import Navbar from './components/Navbar';
import Footer from './components/Footer';

export default {
  components: {
    Navbar,
    Footer
  },
data() {
  return {
    logo_src: "/img/logo.png",
    app_name: "make you burguer"
  }
}

}

</script>

<style>

* {
  font-family: helvetica;
  padding: 0;
  margin: 0;
  box-sizing: border-box;

}

.main-container {

  margin: 50px;
  min-height: 250px;
}

h1 {
  text-align: center;
  font-size: 47px;
  margin-bottom: 30px;
  color: #222;
}
</style>



