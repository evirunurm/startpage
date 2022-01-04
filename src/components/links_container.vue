<template>
<div class="links-container">
  <h3 class="links-folder-name font--light">_{{ name }}</h3>
  <div class="links font--light">
    <p v-for="link in links" class="link"><a :href=link.link>{{ link.name }}</a></p>

  </div>

</div>
</template>

<script>
export default {
  props: {
    name: String,
    links: Array,
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

a {
  color: var(--white);
  text-decoration: none;
}
a:hover {
  text-decoration: line-through;
}
</style>
