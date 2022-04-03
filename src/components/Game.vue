<template>
  <div class="principal-div">
    <div class="row justify-center">
      <div class="col-1" />
      <div class="col-3">
        <div class="text-h6 text-center q-pa-md">{{ title }}</div>
      </div>
      <div class="col-1" />
    </div>
    <div class="row justify-center">
      <div class="col-1">
        <colors-component :colors="colors" @select-color="selectColor" />
      </div>
      <div class="col-3">
        <q-card class="my-card">
          <div class="q-pt-md q-pr-sm q-pb-md q-pl-md">
            <row-component
              v-for="(n, index) in max_guesses"
              :key="index"
              :ref="`row${index}`"
              :active="currentPosition == index"
              :index="index"
              :num-slots="numSlots"
              :selected-color="selectedColor"
              :guess="guesses[index]"
              @change-row-value="currentRowIsClear"
            />
          </div>
        </q-card>
      </div>
      <div class="col-1" :class="status != 'running' ? 'self-center' : 'self-start'">
        <div v-if="status == 'running'" class="q-pa-md">
          <p>
            <q-btn
              :disable="currentRowCleared"
              push
              round
              color="white"
              text-color="positive"
              icon="done"
              @click="sendGuess"
            />
          </p>
          <p>
            <q-btn
              push
              round
              color="white"
              text-color="grey"
              icon="refresh"
              @click="refreshRow"
            />
          </p>
        </div>
        <p v-if="status == 'won' || status == 'lost'" class="text-h6 q-pl-sm" :class="status == 'won' ? 'victory-color': 'lost-color'">
          {{ status == 'won' ? 'VICTORY!' : 'LOST'}}
        </p>
        <div v-if="status == 'lost'" class="solution-text">
          <p>
            Solution
          </p>
          <p>
            <q-icon
              v-for="color in secretCode"
              :key="color"
              name="circle"
              size="20px"
              :color="color"
            />
          </p>
        </div>
      </div>
    </div>
    <div class="row justify-center">
      <div class="col-1" />
      <div class="col-3" />
      <div class="col-1 text-center">
        <q-btn
          flat
          rounded
          no-caps
          color="white"
          text-color="primary"
          label="New game"
          @click="newGame"
        />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import RowComponent from 'src/components/Row.vue';
import ColorsComponent from 'src/components/Colors.vue';
import { api } from '../boot/axios';

export default defineComponent({
  name: 'GameComponent',
  components: { RowComponent, ColorsComponent },
  props: {
    title: {
      type: String,
      required: true
    }
  },

  data() {
    return {
      id: null,
      max_guesses: 0,
      colors: [],
      status: '',
      selectedColor: null,
      numSlots: 0,
      guesses: [],
      secretCode: [],
      currentRowCleared: true
    }
  },

  computed: {
    currentPosition() {
      return Object.keys(this.guesses).length;
    },

    currentRow() {
      let currentRow = (this.$refs[`row${this.currentPosition}`] as InstanceType<typeof Array>)[0] as InstanceType<typeof RowComponent>;
      return currentRow;
    }
  },
  
  methods: {
    newGame() {
      this.clearPointer();
      this.max_guesses = 0;
      let text = { num_colors: 6, num_slots: 4 };
      api.post('/api/games/', text).then(({ data }) => {
        this.id = data.id;
        this.colors = data.colors;
        this.max_guesses = data.max_guesses;
        this.numSlots = data.num_slots;
        this.status = data.status;
        this.guesses = data.guesses;
        this.secretCode = data.secret_code;
        this.currentRowCleared = true;
      })
    },

    selectColor(color: null) {
      this.selectedColor = color;
    },

    sendGuess() {
      let solution = {
        code: this.currentRow.rowValue
      };
      api.post(`/api/games/${this.id}/guesses/`, solution).then(({ data }) => {
        this.colors = data.colors;
        this.max_guesses = data.max_guesses;
        this.numSlots = data.num_slots;
        this.status = data.status;
        this.guesses = data.guesses;
        this.clearPointer();
      })
    },

    refreshRow() {
      this.currentRow.refreshRow();
      this.clearPointer();
    },
    
    clearPointer() {
      this.selectedColor = null;
      document.body.style.cursor = 'pointer';
      this.currentRowIsClear();
    },

    currentRowIsClear() {
      this.currentRowCleared = this.$refs.row0 ? this.currentRow.rowValue.includes('grey-4') : true;
    }
  }
});
</script>
