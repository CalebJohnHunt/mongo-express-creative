<template>
    <div class='palette'>
        <div v-if='this.$root.$data.palette.length > 0'>
            <button class='toggle-codes' @click='toggleCodes()'>Turn {{ this.showCodes ? "off" : "on" }} codes</button>
            <div class='swatch' v-for='swatch in this.$root.$data.palette' :key='swatch.id'>
                <div class='name'>
                    <h1>{{ swatch.name }}</h1>
                </div>
                <div class='color-wrapper'>
                    <div class='color-code-wrapper' v-for='color in swatch.colors' :key='color' :style='{"background-color": color, width: 100/swatch.colors.length + "%"}'>
                        <!-- <div class='color' :style='{"background-color": color}'></div> -->
                        <div class='color-code'>{{ showCodes ? color : ""}}</div>
                        <!-- <div class='color-code'></div> -->
                    </div>
                </div>
                <button class='removeButton' @click='removeSwatch(swatch)'>X</button>
            </div>
        </div>
        <div v-else>
            <h2>Your swatches will show up here!</h2>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Palette',
    data: () => { return {
        showCodes: false,
    }},
    methods: {
        removeSwatch(swatch) {
            let index = this.$root.$data.palette.findIndex(el => el.id == swatch.id);
            if (swatch.added) swatch.added = false;
            this.$root.$data.palette.splice(index, 1);
        },
        toggleCodes() {
            this.showCodes = !this.showCodes;
        }
    }
}
</script>

<style scoped>
.swatch {
    display: grid;
    margin: 10px 0;
    /* border: 1px solid black; */
    grid-template-columns: 20% 70% 10%;
    grid-template-areas: "name colors button";
}

.name {
    grid-area: "name";
    word-wrap: break-word;
    /* width: 400px; */
}

h1 {
    font-weight: bold;
    font-size: 20px;
}

.color-wrapper {
    grid-area: colors;
    display: flex;
    flex-direction: row;
    width: 100%;
}

.color-code-wrapper {
    width: 100%;
    min-height: 60px;
    display: flex;
    flex-direction: column;
}

.color-code {
    color: white;
    text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000, 1px 1px 0 #000;
    font-size: 14px;
    letter-spacing: 1px;
    word-wrap: break-word;
}

.color {
    width: 100%;
    height: 100%;
}

.toggle-codes {
    border: 1px solid black;
    min-height: 25px;
    font-size: 15px;
}

.removeButton {
    border: 1px solid black;
    grid-area: button;
}
</style>