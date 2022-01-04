<template>
<div class="links-container">
  <article class="links-folder" @mouseover="hover = true"  @mouseleave="hover = false">
    <h3 class="links-folder-name font--light" >_{{ name }}</h3>
    <div class="links-folder--edit" v-if="hover" @click="isEditing = true">
    </div>
    <section class="folder-editor" v-if="isEditing">
      <input v-model="name" type="text">
      <link_editor v-on:deleteLink="deleteLink" v-for="link in links" :link=link :isEditingLink=isEditingLink />
      <div class="add-link" id="addLink" @click="startAddingLink"></div>
      <div class="editor--save" @click="saveOptions"></div>
    </section>
  </article>

  <article class="links font--light">
    <p v-for="link in links" class="link"><a :href="link.url">{{ link.name }}</a></p>
  </article>

</div>
</template>

<script>
import link_editor from "./link_editor.vue";

export default {
  props: {
    index: Number,
    name: String,
    links: Array,
  },
  data() {
    return {
      hover: false,
      isEditing: false,
      isEditingLink: false,
    }
  },
  methods: {
    saveOptions() {
      this.isEditing = false;
      this.$emit('modifyShortcuts', this.index, this.name, this.links)
    },
    deleteLink(link) {
      this.links.splice(this.links.indexOf(link), 1);
      console.log(this.links)
    },
    startAddingLink() {
      this.links.push({
        name: "name",
        url: "http://..."
      });

    }
  },
  components: {
    link_editor,
  }
}
</script>

<style scoped>

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

.links-container {
  display: flex;
  flex-direction: column;
}

.links-folder {
  padding: 2rem;
  display: flex;
  position: relative;
}

.links-folder-name {
  color: var(--pink);
}

.links-folder--edit {
  height: 20px;
  width: 20px;
  background: red;
  position: absolute;
  left: 80%;

}

.folder-editor {
  background: var(--grey);
  position: absolute;
  left: 80%;
  z-index: 10;
  box-shadow: 0px 5px 50px 2px rgba(0,0,0,0.3);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 1rem;
}

.editor--save {
  width: 20px;
  height: 20px;
  background: green;
align-self: center;
}

.add-link {
  width: 20px;
  height: 20px;
  background: pink;
  align-self: center;
}

</style>
