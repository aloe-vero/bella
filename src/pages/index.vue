<template>
  <v-app>
    <MusicPlayer />
    <MessageHeader />
    <ScrollImage />

  </v-app>
</template>

<script>
import MusicPlayer from "@/components/MusicPlayer.vue";
import ScrollImage from "@/components/ScrollImage.vue";

import MessageHeader from "@/components/MessageHeader.vue";


export default {
  components: {
    MessageHeader,
  },
};

</script>



<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

* {
  font-family: "Montserrat", sans-serif;
}
</style>