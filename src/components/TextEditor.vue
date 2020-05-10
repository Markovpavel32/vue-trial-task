<template>
  <div>
    <c-text v-for="item in start_list" :item="item" :key="item.text"
            @make-active="make_active(item)"
            @change-color="change_color()"
            @change-font-size="change_font_size()"
            @change-background="change_background()"
            @save-text="save_text"
    ></c-text>
    <div>
      <button @click="console_JSON">console JSON</button>
    </div>
  </div>
</template>

<script>
import CText from "@/components/CText";
import {TEXT_EDITOR_COLORS, TEXT_EDITOR_FONT_SIZES, TEXT_EDITOR_BACKGROUND_COLORS} from './CONSTANTS'

const next_element = (value, arr) => {
  const index = arr.findIndex(x => x === value)
  if (index === arr.length - 1) return arr[0]
  return arr[index + 1]
};

export default {
  name: 'text-editor',
  components: {CText},
  data() {
    return {
      active_element: null,
      start_list: [
        {
          text: 'Hi\n',
          fontSize: TEXT_EDITOR_FONT_SIZES[0],
          color: TEXT_EDITOR_COLORS[0],
          borderRadius: '4px',
          backgroundColor: TEXT_EDITOR_BACKGROUND_COLORS[0],
          active: false,
        },
        {
          text: 'My lovely',
          fontSize: TEXT_EDITOR_FONT_SIZES[0],
          color: TEXT_EDITOR_COLORS[0],
          backgroundColor: TEXT_EDITOR_BACKGROUND_COLORS[0],
          borderRadius: '4px',
          active: false,
        },
        {
          text: 'little',
          fontSize: TEXT_EDITOR_FONT_SIZES[1],
          color: TEXT_EDITOR_COLORS[1],
          active: false,
          backgroundColor: 'transparent'
        },
        {
          text: 'Ponny',
          fontSize: TEXT_EDITOR_FONT_SIZES[0],
          color: TEXT_EDITOR_COLORS[2],
          backgroundColor: TEXT_EDITOR_BACKGROUND_COLORS[1],
          active: false
        },
      ]
    }
  },

  methods: {
    make_active(item) {
      this.start_list.forEach((x, i) => {
        x.active = false;
        if (x.text === item.text) {
          x.active = true
          this.active_element = i
        }
      });
    },
    change_color() {
      const color = this.start_list[this.active_element].color;
      this.start_list[this.active_element].color = next_element(color, TEXT_EDITOR_COLORS)
    },
    change_font_size() {
      const font_size = this.start_list[this.active_element].fontSize;
      this.start_list[this.active_element].fontSize = next_element(font_size, TEXT_EDITOR_FONT_SIZES)
    },
    change_background() {
      const background = this.start_list[this.active_element].backgroundColor;
      this.start_list[this.active_element].backgroundColor = next_element(background, TEXT_EDITOR_BACKGROUND_COLORS);
      if(this.start_list[this.active_element].backgroundColor === 'transparent')  delete this.start_list[this.active_element].borderRadius;
      else this.start_list[this.active_element].borderRadius = '4px'
    },
    save_text(value) {
      this.start_list[this.active_element].text = value
    },
    console_JSON() {
      let arr = [...this.start_list].map(x => {
        let obj = {...x};
        obj.text = obj.text.replace(/\n/g, '');
        delete obj.active;
        return obj
      });
      console.log(JSON.stringify(arr))
    }
  }
}
</script>
