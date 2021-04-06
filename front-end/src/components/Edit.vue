<template>
<div>
  {{ paletteID }}
  <p/>
  {{ swatchID }}
</div>
</template>

<script>
export default {
  props: {
    paletteID: String,
    swatchID: String,
  },
}
</script>
