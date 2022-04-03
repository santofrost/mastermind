<template>
      <q-icon :name="iconName" size="12px" :color="iconColor" />
</template>

<script lang="ts">
import { defineComponent } from 'vue';

const status = {
  grey: 0,
  black: 1,
  white: 2
};

const colors = {
  black: 'black',
  grey: 'grey-4'
};

const icons = {
  circle: 'circle',
  borderCircle: 'radio_button_unchecked'
};

export default defineComponent({
  name: 'MyIconComponent',
  props: {
    status: {
      type: Number,
      required: false,
      default: 0
    }
  },
  
  computed: {
    iconName() {
      return this.status == status.white ? icons.borderCircle : icons.circle;
    },

    iconColor() {
      return this.status == status.black ? colors.black : colors.grey;
    }
  }
});
</script>
