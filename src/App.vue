<template>
  <v-app>
    <v-container>
      <text-tags :tags="tags" alignment="left" style="margin-bottom: 20px;"></text-tags>
      <text-tags :tags="tags" alignment="justify" style="margin-bottom: 20px;"></text-tags>
      <text-tags :tags="tags" alignment="left" style="width: 50%; margin-bottom: 20px;"></text-tags>
      <text-tags :tags="tags" alignment="justify" style="width: 50%; margin-bottom: 20px;"></text-tags>
    </v-container>
  </v-app>
</template>

<script>
import TextTags from './components/TextTags.vue';

export default {
  name: 'App',
  components: {
    TextTags
  },
  data() {
    return {
      tags: [
        {text: "4.3", icon: "mdi-star"},
        {text: "Фридландские ворота"},
        {text: "1,2 км от вас"}
      ]
    }
  }
}
</script>
