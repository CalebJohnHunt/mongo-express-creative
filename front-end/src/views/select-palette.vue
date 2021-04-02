<template>
  <div class='content'>
    <div v-for='palette in palettes' :key='palette._id'>
      <div @click='click(palette)'>{{ palette._id }} </div>
      <div>{{ palette.name }} </div>
      <div>{{ palette.creationDate }} </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'SelectPalette',
  data: () => { return {
    palettes: [],
  }},
  created() {
    this.getPalettes();
  },
  methods: {
    click(palette) {
      console.log(palette);
    },
    select(paletteID) {
      this.$root.$data.selectedPalette = paletteID;
    },
    async getPalettes() {
      try {
        const response = await axios.get('/api/palettes');
        this.palettes = response.data;
      } catch (error) {
        console.log(error);
      }
    },
  }
}

</script>