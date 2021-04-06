<template>
  <div>
    <div v-if='paletteID != 0'>
      <div v-if='swatchID != 0'>
        <EditComp :paletteID="this.paletteID" :swatchID="this.swatchID" />
      </div>
      <!-- Select a swatch from palette -->
      <div v-else>
        <div v-for='swatch in this.swatches' :key='swatch._id'>
          {{ swatch.name }}
          <button @click='editSwatch(swatch)'>Edit</button>
        </div>
      </div>
    </div>
    <div v-else>
      No palette
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import EditComp from '../components/Edit.vue';
export default {
  name: 'Edit',
  data: () => { return {
    paletteID: '0',
    swatchID: '0',
    swatches: [],
  }},
  components: {
    EditComp
  },
  async created() {
    this.paletteID = this.$root.$data.selectedPaletteID.toString();
    if (this.paletteID == 0) {
      console.log("No palette");
      return;
    }
    try {
      const response = await axios.get('/api/palettes/' + this.paletteID + '/swatches');
      this.swatches = response.data;
    } catch (error) {
      console.log(error);
    }
    },
  methods: {
    editSwatch(swatch) {
      this.swatchID = swatch._id;
    }
  },
}
</script>