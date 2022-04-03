<template>
  <q-list>
    <q-item v-for="color in colors" :key="color">
      <q-item-section />
      <q-item-section avatar>
        <q-icon name="circle" size="32px" :color="color" @click="selectColor(color)" />
      </q-item-section>
    </q-item>
  </q-list>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import { Todo } from './models';

interface ICursors {
  red: string,
  blue: string,
  green: string,
  yellow: string,
  orange: string,
  white: string
};

const cursors: ICursors = {
  red: "url('http://www.rw-designer.com/cursor-extern.php?id=93173'), auto",
  blue: "url('http://www.rw-designer.com/cursor-extern.php?id=93177'), auto",
  green: "url('http://www.rw-designer.com/cursor-extern.php?id=93176'), auto",
  yellow: "url('http://www.rw-designer.com/cursor-extern.php?id=93175'), auto",
  orange: "url('http://www.rw-designer.com/cursor-extern.php?id=93174'), auto",
  white: 'url(http://www.rw-designer.com/cursor-extern.php?id=93179), auto'
};

export default defineComponent({
  name: 'ColorsComponent',
  props: {
    colors: {
      type: Array as PropType<Todo[]>,
      default: () => []
    }
  },
  
  methods: {
    selectColor(color: string) {
      document.body.style.cursor = cursors[color as keyof ICursors];
      this.$emit('selectColor', color);
    }
  }
});
</script>
