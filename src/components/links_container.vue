<template>
<div class="links-container">
  <article class="links-folder" @mouseover="hover = true"  @mouseleave="hover = false">
    <h3 class="links-folder-name font--light" >_{{ name }}</h3>
    <div class="links-folder--edit" v-if="hover" @click="isEditing = true">
      <svg width="20" height="20" viewBox="0 0 70 70" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path fill-rule="evenodd" clip-rule="evenodd" d="M43.1858 12.565C44.7479 11.0029 47.2806 11.0029 48.8427 12.565L57.5417 21.264C59.1038 22.8261 59.1038 25.3588 57.5417 26.9209L26.888 57.5745C25.3259 59.1366 22.7933 59.1366 21.2312 57.5745L12.5322 48.8755C10.9701 47.3134 10.9701 44.7808 12.5322 43.2187L43.1858 12.565ZM21.0175 46.0471L24.0596 49.0892L49.0564 24.0924L46.0142 21.0503L21.0175 46.0471Z" fill="white"/>
        <path d="M63.695 15.3934L54.6064 6.30492L55.0829 5.82843C56.645 4.26633 59.1777 4.26633 60.7398 5.82843L64.1715 9.2601C65.7336 10.8222 65.7336 13.3549 64.1715 14.917L63.695 15.3934Z" stroke="white" stroke-width="8" stroke-linecap="round" stroke-linejoin="round"/>
        <path d="M14.8443 61.8689L8.13115 54.9743L4 66L14.8443 61.8689Z" fill="white" stroke="white" stroke-width="8" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
    <section class="folder-editor" v-show="isEditing">
      <input class="input-name" v-model="name" type="text" maxlength="20">
      <link_editor v-on:deleteLink="deleteLink" v-for="link in links" :link=link />
      <div class="add-link" id="addLink" @click="addLink"></div>
      <div class="editor--save" @click="saveOptions">
        <svg width="30" height="30" viewBox="0 0 62 62" fill="none" xmlns="http://www.w3.org/2000/svg">
          <rect x="4" y="4" width="54" height="54" rx="12" stroke="white" stroke-width="8"/>
          <path d="M19 30.5L28.5 40L43.5 25" stroke="white" stroke-width="8" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </div>
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
    },
    addLink() {
      if (this.links.length <= 10) {
        this.links.push({
          name: "name",
          url: "https://..."
        });
      } else {
        alert("There's a maximum amount of 10 links per section, sorry:))");
      }
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
  height: 30px;
  width: 30px;
  position: absolute;
  right: -10px;
  cursor: pointer;
  opacity: 30%;
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

.input-name {
  font-size: 1rem;
  padding: 0.25rem 0.25rem 0.25rem 0;
  background: none;
  border: none;
  border-bottom: 1px solid var(--white);
  color: var(--white);
  margin-bottom: 1rem;
}
.input-name:focus {
  outline: none;
}

</style>
