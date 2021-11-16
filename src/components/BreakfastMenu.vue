<template>
  <div
    class="
      max-w-sm
      py-4
      rounded
      overflow-hidden
      shadow-lg
      bg-white
      text-center
      relative
    "
  >
    <button
      @click="sendCart"
      class="absolute top-0 left-0 mt-2 mr-2 text-gray-600 hover:text-green-600"
    >
      <svg
        class="fill-current"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        aria-hidden="true"
        focusable="false"
        width="2em"
        height="2em"
        style="
          -ms-transform: rotate(360deg);
          -webkit-transform: rotate(360deg);
          transform: rotate(360deg);
        "
        preserveAspectRatio="xMidYMid meet"
        viewBox="0 0 1024 1024"
      >
        <path
          d="M696 480H544V328c0-4.4-3.6-8-8-8h-48c-4.4 0-8 3.6-8 8v152H328c-4.4 0-8 3.6-8 8v48c0 4.4 3.6 8 8 8h152v152c0 4.4 3.6 8 8 8h48c4.4 0 8-3.6 8-8V544h152c4.4 0 8-3.6 8-8v-48c0-4.4-3.6-8-8-8z"
        ></path>
        <path
          d="M512 64C264.6 64 64 264.6 64 512s200.6 448 448 448 448-200.6 448-448S759.4 64 512 64zm0 820c-205.4 0-372-166.6-372-372s166.6-372 372-372 372 166.6 372 372-166.6 372-372 372z"
        ></path>
      </svg>
    </button>

    <!-- ONLY SHOW IF ADDED TO CART -->
    <button
      v-if="findCartByName"
      @click="sendRemoveCart"
      class="absolute top-0 right-0 mt-2 mr-2 text-gray-600 hover:text-red-600"
    >
      <svg
        class="fill-current"
        viewBox="0 0 20 20"
        width="1.9em"
        height="1.9em"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M10 20c5.523 0 10-4.477 10-10S15.523 0 10 0 0 4.477 0 10s4.477 10 10 10zm0-2a8 8 0 100-16 8 8 0 000 16zm5-9v2H5V9h10z"
          fill-rule="evenodd"
        ></path>
      </svg>
    </button>
    <img class="mx-auto" :src="image" alt="Pancakes" />
    <div class="px-6 pt-4">
      <div class="font-bold text-xl mb-2">
        {{ name }} ({{ findCartByName ? findCartByName.quantity : 0 }})
      </div>
    </div>
    <div class="px-6 py-4">
      <span
        v-for="group in groups"
        :key="group"
        class="
          inline-block
          bg-gray-200
          rounded-full
          px-3
          py-1
          text-sm
          font-semibold
          text-gray-700
          mr-2
          mb-2
        "
        >{{ group }}</span
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "BreakfastMenu",
  data() {
    return {};
  },
  props: {
    name: String,
    image: String,
    groups: Array,
    count: Number,
  },
  emits: ["add-cart", "remove-cart"],
  inject: ["cartProvide"],
  methods: {
    sendCart() {
      this.$emit("add-cart", this.name);
    },
    sendRemoveCart() {
      // console.log(this.name);
      this.$emit("remove-cart", this.name);
    },
  },
  computed: {
    findCartByName() {
      return this.cartProvide.find(
        ({ name }) => name.toLowerCase() === this.name.toLowerCase()
      );
    },
  },
  beforeUpdate() {
    // this.findCartByName;
    // console.log(this.findCartByName);
  },
  updated() {
    // this.findCartByName;
  },
};
</script>