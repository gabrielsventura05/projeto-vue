<template>
  <Banner />
  <div class="main-container">
<h1>Monte seu Hamburguer</h1>
<BurguerForm />

   </div>
</template>

<script>

import Banner from '../components/Banner';
import BurguerForm from '../components/BurguerForm';

export default {
  name: "Home",
  components: {
    Banner,
    BurguerForm
  }
}

</script>
