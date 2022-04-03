<template>
  <div class="row items-center">
    <div class="col-3" />
    <div class="col-3">
      <my-icon-component :status="values[0]" />
    </div>
    <div class="col-3">
      <my-icon-component :status="values[1]" />
    </div>
    <div class="col-3" />
    
    <div class="col-3" />
    <div class="col-3">
      <my-icon-component :status="values[2]" />
    </div>
    <div class="col-3">
      <my-icon-component :status="values[3]" />
    </div>
    <div class="col-3" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import MyIconComponent from 'src/components/MyIcon.vue';

const status = {
  grey: 0,
  black: 1,
  white: 2
};

export default defineComponent({
  name: 'RowResultComponent',
  components: { MyIconComponent },
  props: {
    guess: {
      type: Object,
      required: false
    }
  },

  data() {
    return {
      values: [0, 0, 0, 0]
    }
  },

  watch: {
    guess() {
      this.setColors();
    }
  },
  
  methods: {
    setColors() {
      let res = [];
      if (this.guess) {
        let blacks = this.setBlacks();
        let whites = this.setWhites();
        
        res = blacks.concat(whites);
      }
      this.values = res;
    },

    setBlacks() {
      let amount = this.guess?.black_pegs;
      return new Array(amount).fill(status.black);
    },

    setWhites() {
      let amount = this.guess?.white_pegs;
      return new Array(amount).fill(status.white);
    },
  }
});
</script>
