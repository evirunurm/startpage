<template>
  <div>
    <div class="link-title">
      <p>{{ link.name }}</p>
      <div class="link-title--options">
        <div class="div" @click="deleteLink();" ></div>
        <div class="div div--green" @click="isEditingLink=!isEditingLink" ></div>
      </div>
    </div>
    <div class="link-details" v-show="isEditingLink">
      <input v-model="link.name" type="text">
      <input v-model="link.url" type="text">
    </div>
  </div>
</template>

<script>
export default {
  name: "link_editor.vue",
  props: {
    link: Object,
    isEditingLink: Boolean,
  },
  data() {
    return {
      /*sEditing: isEditingLink,*/
    }
  },
  methods: {
    deleteLink() {
      this.$emit("deleteLink", this.link);
    }
  }
}
</script>

<style scoped>
.div {
  height: 20px;
  width: 20px;
  background: red;
}

.link-title {
  display: flex;
  justify-content: space-between;
}

.link-details input {
 width: 100%;
}

.link-title--options {
  display: flex;
}

.div--green {
  background: green;
}
</style>