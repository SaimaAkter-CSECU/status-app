<template>
  <v-app>
    <v-main>
      <Header />
      <StatusBox />
    </v-main>
  </v-app>
</template>

<script>
import StatusBox from './components/StatusBox';
import Header from './components/Header'; 

export default {
  name: 'App',

  components: {
    StatusBox,
    Header
  }
};
</script>
