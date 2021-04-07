<template>
<div class='content'>
  <div class='form'>
    <input type='text' v-model='name' placeholder='Palette name' />
    <button :class='{favorited: this.isFavorite}' @click='isFavorite = !isFavorite'>&#10084;</button>
    <!-- Instead of a checkbox, use an outline and filled in star to show favorite -->
    <button @click='submitPalette()'>Add palette</button>
  </div>

  <div class='palettes'>
    <div class='palette' v-for='palette in palettes' :key='palette._id' :class='{selected : selectedP == palette._id}'>
      <button :class='{favorited: palette.isFavorite}' @click='toggleFavorite(palette)'>&#10084;</button>
      <button class='select-button' @click='selectP(palette._id)'>Select</button> 
      <div class='creation-date'>{{ palette.creationDate.slice(0, 10) }}</div>
      <input v-model='palette.name' type='text'>
      <button @click='renamePalette(palette)'>Rename</button>
      <button @click='deletePalette(palette._id)'>X</button>
    </div>
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
    async selectP(paletteID) {
      this.$root.$data.selectedPaletteID = paletteID;
      this.selectedP = paletteID;
    },
    async toggleFavorite(palette) {
      try {
        await axios.put('/api/palettes/' + palette._id, {
          name: palette.name,
          creationDate: palette.creationDate,
          isFavorite: !palette.isFavorite,
        });
        this.getPalettes();
      } catch (error) {
        console.log(error);
      }
    },
    async renamePalette(palette) {
      try {
        await axios.put('/api/palettes/' + palette._id, {
          name: palette.name,
          creationDate: palette.creationDate,
          isFavorite: palette.isFavorite,
        });
        this.getPalettes();
      } catch (error) {
        console.log(error);
      }
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
      // Put favorites first, then append the rest
      try {
        const response = await axios.get('/api/palettes');
        let allPalettes = response.data;
        let favorites = allPalettes.filter(el => el.isFavorite);
        let nonfavorites = allPalettes.filter(el => !el.isFavorite);
        this.palettes = favorites.concat(nonfavorites);
        this.selected = this.$root.$data.selectedPaletteID;
      } catch (error) {
        console.log(error);
      }
    },
    async deletePalette(paletteID) {
      try {
        await axios.delete('/api/palettes/' + paletteID);
        if (paletteID === this.$root.$data.selectedPaletteID) {
          this.$root.$data.selectedPaletteID = 0;
          this.selectedP = 0;
        }
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

.palettes {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.favorited {
  color: red !important;
}

.creation-date {
  text-align: center;
  margin: auto;
}

</style>