<template>
    <div class='content'>
        <div class='swatch-wrapper' v-for='swatch in this.$root.$data.swatches' :key='swatch.id'>
            <div class='name'> {{ swatch.name }} </div>
                <div class='swatch' v-for='c in swatch.colors' :key='c' :style='{"background-color": c}'>
                    <div class='swatch-color'>{{ c }}</div>
                </div>
            <div class='added-button'>
                <button v-if='!swatch.added' @click='addSwatch(swatch)'>Add to Palette</button>
                <div v-else>Added!</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'Swatches',
  methods: {
      styleBackgroundColor(c) {
          return 'background-color: ' + c;
      },
      addSwatch(swatch) {
          swatch.added = !swatch.added;
          if (swatch.added) {
              this.$root.$data.palette.push(swatch);
          } else {
              const index = this.$root.$data.palette.findIndex((el) => el.id == swatch.id);
              this.$root.$data.palette.splice(index, 1);
          }
      }
  }
}
</script>


<style scoped>
.content {
    max-width: 800px;
    margin: auto;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    align-items: center;
}

.swatch-wrapper {
    display: flex;
    flex-direction: column;
    min-width: 150px;
    /* width: 40%; */
    max-width: 400px;

    margin: 10px 10px;
    /* border: 1px solid #1099b9; */
    border: 1px solid black;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;

    /* background-color: #1099b9; */
}

.name {
    font-weight: bold;
    font-size: 20px;
    /* background-color: #1099b9; */
}

.swatch {
    color: #fff;
    text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000, 1px 1px 0 #000;
    letter-spacing: 1px;
    min-height: 30px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.added-button {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    /* color: #ddd; */
    height: 30px;
}

.added-button button {
    width: 100%;
    height: 100%;
    border: 1px solid black;
    /* border-top: 2px solid black; */
    font-size: 16px;
    color: white;
}
</style>