<template>
  <div class="links-container">
    <div class="link-title">
      <p v-show="!isEditing" @dblclick="isEditing = true;selectNameInput();">{{ link.name }}</p>
      <div class="link-title--options">
        <button v-show="!isEditing" class="edit-link" @click="isEditing=!isEditing">
          <svg width="18" height="18" viewBox="0 0 70 70" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M43.1858 12.565C44.7479 11.0029 47.2806 11.0029 48.8427 12.565L57.5417 21.264C59.1038 22.8261 59.1038 25.3588 57.5417 26.9209L26.888 57.5745C25.3259 59.1366 22.7933 59.1366 21.2312 57.5745L12.5322 48.8755C10.9701 47.3134 10.9701 44.7808 12.5322 43.2187L43.1858 12.565ZM21.0175 46.0471L24.0596 49.0892L49.0564 24.0924L46.0142 21.0503L21.0175 46.0471Z" fill="white"/>
            <path d="M63.695 15.3934L54.6064 6.30492L55.0829 5.82843C56.645 4.26633 59.1777 4.26633 60.7398 5.82843L64.1715 9.2601C65.7336 10.8222 65.7336 13.3549 64.1715 14.917L63.695 15.3934Z" stroke="white" stroke-width="8" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M14.8443 61.8689L8.13115 54.9743L4 66L14.8443 61.8689Z" fill="white" stroke="white" stroke-width="8" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
        <button v-show="isEditing" class="edit-link" @click="isEditing=!isEditing">
          <svg width="20" height="20" viewBox="0 0 51 35" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M4 13.9L20.6735 31L47 4" stroke="white" stroke-width="8" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
        <button class="edit-link" @click="deleteLink();" >
          <svg width="15" height="15" viewBox="0 0 39 39" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M4 4L35 35" stroke="white" stroke-width="8" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M35 4L4 35" stroke="white" stroke-width="8" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>
    </div>
    <div class="link-details" v-show="isEditing">
      <input id="linkName" v-model="link.name" type="text" maxlength="20" >
      <input v-model="link.url" type="text" maxlength="20">
    </div>
  </div>
</template>

<script>
export default {
  name: "link_editor.vue",
  props: {
    link: Object,
  },
  data() {
    return {
      isEditing: false,
    }
  },
  methods: {
    deleteLink() {
      this.$emit("deleteLink", this.link);
    },
    selectNameInput() {
      document.getElementById('linkName').focus();
    }
  }
}
</script>

<style scoped>
button {
  background: none;
  border: none;
}

.link-title {
  display: flex;
  justify-content: space-between;
}

.link-title--options {
  display: flex;
  position: absolute;
  right: 1.25rem;
}

.edit-link {
  cursor: pointer;
  opacity: 20%;
  padding: 0 0.25rem;
}

.edit-link:hover {
  opacity: 80%;
}

.link-details {
  margin-bottom: 1rem;
}

.link-details input {
  background: none;
  border: none;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  padding: 0.5rem 0;
  color: var(--fontColor);
  width: 100%;
  font-size: 1rem;
}

.link-details input:focus {
  outline: none;
  background: rgba(0, 0, 0, 0.1);
  border-bottom: 1px solid rgba(255, 255, 255, 0.8);
}
</style>