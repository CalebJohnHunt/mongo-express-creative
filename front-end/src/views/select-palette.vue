<template>
  <div class='content'>
    <div class='form'>
      <input type='text' v-model='name' placeholder='Palette name' />
      <input type='checkbox' v-model='isFavorite' />
      <!-- Instead of a checkbox, use an outline and filled in star to show favorite -->
      <button @click='submitPalette()'>Add palette</button>
    </div>


    <div class='palette' v-for='palette in palettes' :key='palette._id' :class='{selected : selectedP == palette._id}'>
      <button class='select-button' @click='selectP(palette._id)'>Select</button> 
      <input v-model='palette.name' type='text'>
      <div>{{ palette.creationDate }}</div>
      <button @click='deletePalette(palette._id)'>X</button>
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
    name: '',
    isFavorite: false,
  }},
  created() {
    this.getPalettes();
    this.selectedP = this.$root.$data.selectedPaletteID;
  },
  computed: {
  },
  methods: {
    click(palette) {
      console.log(palette);
    },
    selectP(paletteID) {
      this.$root.$data.selectedPaletteID = paletteID;
      this.selectedP = paletteID;
    },
    async submitPalette() {
      if (!this.name) {
        console.log("Name needed");
        return;
      }
      try {
          await axios.post('/api/palettes', {
          name: this.name,
          isFavorite: this.isFavorite,
          creationDate: new Date(),
        });
        this.name = '';
        this.isFavorite = false;
        this.getPalettes()
      } catch (error) {
        console.log(error);
      }
    },
    async getPalettes() {
      try {
        const response = await axios.get('/api/palettes');
        this.palettes = response.data;
        this.selected = this.$root.$data.selectedPaletteID;
      } catch (error) {
        console.log(error);
      }
    },
    async deletePalette(paletteID) {
      try {
        await axios.delete('/api/palettes/' + paletteID);
        this.$root.$data.selectedPaletteID = 0;
        this.selectedP = 0;
        // console.log(response);
        this.getPalettes();
      } catch (error) {
        console.log(error);
      }
    }
  }
}

</script>

<style scoped>
.selected {
  border: 1px solid red;
}

.palette {
  display: flex;
  flex-direction: row;
}
</style>