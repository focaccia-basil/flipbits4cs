<template>
  <div class="container">
    <div class="bit-pattern">
      <bit
        v-for="idx in maxBits - 1"
        :key="idx"
        :interactive="interactive"
        :bit="bits[idx]"
        :index="idx"
        :maxBits="maxBits"
        :random="random"
        :donorList="donorList"
        @flipped-bit="updateBitValue"
        class="bit"
      ></bit>
    </div>
  </div>
</template>

<script>
import Bit from './Bit.vue';
export default {
  components: { Bit },
  props: ['number', 'interactive', 'random', 'donorList'],
  emits: ['update-bit-value'],
  data() {
    return {
      bits: [],
      bitValue: 0,
      maxBits: 0,
    };
  },
  watch: {
    bitValue() {},
  },
  methods: {
    parseBits(number) {
      this.bitValue = number;
      var mask = 1;
      var count = this.maxBits - 1;
      while (count > 0) {
        const bit = number & mask;
        this.bits[count] = bit > 0 ? 1 : 0;
        mask <<= 1;
        count -= 1;
      }
    },
    updateBitValue(payload) {
      if (payload.op == 'sub') {
        this.bitValue -= payload.value;
      } else {
        this.bitValue += payload.value;
      }
      this.$emit('update-bit-value', this.bitValue);
    },
  },
  created() {
    this.maxBits = Math.round(Math.log2(800000) + 1);
    this.bitValue = this.number;
    var i = 0;
    while (i <= this.maxBits) {
      this.bits.push(0);
      i += 1;
    }
    this.parseBits(this.number);
  },
};
</script>

<style scoped>
.bit-pattern {
  margin: 2rem;
}
.bit {
  display:inline;
}
</style>
