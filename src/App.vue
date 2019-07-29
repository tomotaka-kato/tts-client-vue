<template>
  <v-app>
    <v-content>
      <Content/>
    </v-content>
  </v-app>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';
import Content from './components/Content.vue';
@Component({
  components: {
    Content,
  },
})
export default class App extends Vue {}
</script>
