<template>
  <div class='content'>
    <div v-for='palette in palettes' :key='palette._id' @click='selectP(palette._id)' :class='{selected : selectedP == palette._id}'>
      <div >{{ palette._id }} </div>
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
    selectedP: 0,
  }},
  created() {
    this.getPalettes();
  },
  computed: {
  },
  methods: {
    click(palette) {
      console.log(palette);
    },
    selectP(paletteID) {
      this.$root.$data.selectedPalette = paletteID;
      this.selectedP = paletteID;
    },
    async getPalettes() {
      try {
        const response = await axios.get('/api/palettes');
        this.palettes = response.data;
        this.selected = this.$root.$data.selectedPalette;
      } catch (error) {
        console.log(error);
      }
    },
  }
}

</script>

<style scoped>
.selected {
  border: 1px solid red;
}
</style>