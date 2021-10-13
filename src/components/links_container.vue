<template>
<div class="links-container">
  <h3 class="links-folder-name font--light">_general</h3>
  <div class="links font--light">
    <p class="link">whatsapp</p>
    <p class="link">discord</p>
    <p class="link">anilist</p>
    <p class="link">animeflv</p>
  </div>

</div>
</template>

<script>
export default {
  props: {

  },
  data() {
    return {

    }
  }
}
</script>

<style scoped>
.links-container {
  display: flex;
  flex-direction: column;
}

.links-folder-name {
  margin: 0 0 1.5em 0;
  color: var(--pink);
}

.link {
  padding: 0.25em;
}
</style>
