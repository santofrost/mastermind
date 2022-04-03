<template>
  <div class="row items-center">
    <div class="col-1">
      {{ (index + 1).toLocaleString("en-US", { minimumIntegerDigits: 2, useGrouping: false }) }}
    </div>
    <div class="col" v-for="(position, index) in rowValue" :key="index">
      <q-icon name="circle" size="32px" :color="position" class="q-pa-xs" @click="setColor(index)" />
    </div>
    <div class="col">
      <row-result-component :guess="guess" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import RowResultComponent from 'src/components/RowResult.vue';

export default defineComponent({
  name: 'RowComponent',
  components: { RowResultComponent },
  props: {
    index: {
      type: Number,
      required: true
    },
    numSlots: {
      type: Number,
      required: true
    },
    selectedColor: {
      type: String,
      required: false
    },
    guess: {
      type: Object,
      required: false
    },
    active: {
      type: Boolean,
      required: false,
      default: false
    }
  },

  created() {
    this.refreshRow();
  },

  data() {
    return {
      rowValue: [] as Array<string>
    }
  },
  
  methods: {
    setColor(position: number) {
      if (this.active && this.selectedColor && !this.guess){
        this.rowValue[position] = this.selectedColor;
        this.$emit('changeRowValue')
      }
    },

    refreshRow() {
      this.rowValue = new Array(this.numSlots).fill('grey-4');
    }
  }
});
</script>
