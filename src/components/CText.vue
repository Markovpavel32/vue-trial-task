<template>
  <span>
    <span v-if="item.active">
      <button @click="$emit('change-color')">Change color</button>
      <button @click="$emit('change-font-size')">Change font size</button>
      <button @click="$emit('change-background')">Change background</button>
      <button v-if="!is_open" @click="is_open = true">Change text</button>
      <button v-else @click="save_text">Save text</button>
    </span>
    <textarea v-if="is_open" v-model="new_text"></textarea>
    <span v-else @click="$emit('make-active')" v-html="item.text.replace(/\n/g, '<br />') + ' '"
          :style="{
            color: item.color,
            fontSize: item.fontSize,
            borderRadius: item.borderRadius,
            backgroundColor: item.backgroundColor,
          }"
    ></span>
  </span>
</template>

<script>
export default {
  name: "CText",
  props: {
    item: {
      required: true,
      type: Object
    }
  },
  data() {
    return {
      is_open: false,
      new_text: this.item.text
    }
  },

  methods: {
    save_text() {
      this.$emit('save-text', this.new_text);
      this.is_open = false
    }
  }
}
</script>
