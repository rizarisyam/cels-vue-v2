<template>
  <div class="text-gray-900 bg-gray-100 leading-normal">
    <div class="p-4 max-w-5xl mx-auto">
      <img
        class="mx-auto my-8 max-w-full sm:max-w-sm"
        src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/45561/undraw_Ordinary_day_3gk3.svg"
        alt="A drawing of two houses in a neighborhood"
      />
      <h1 class="text-2xl md:text-4xl lg:text-5xl text-center my-4 font-black">
        <span class="concert-underline">Concert Breakfast Studio</span>
      </h1>
      <nav
        class="
          flex flex-col
          md:flex-row
          bg-gray-300
          rounded
          shadow
          overflow-hidden
          p-2
          mb-8
          text-sm
        "
      >
        <label class="sr-only" for="search">Search</label>
        <input
          v-model="query"
          @keypress.enter="searchQuery"
          class="mb-2 md:mb-0 p-2 rounded flex-grow"
          type="search"
          placeholder="Search for foods"
          id="search"
        />
        <select class="mb-2 md:ml-2 md:mb-0" v-model="filterGroup">
          <option value="">Food Groups</option>
          <option v-for="group in uniqueQroups" :key="group" :value="group">
            {{ group }}
          </option>
        </select>
        <button
          class="md:ml-2 p-2 rounded"
          style="background-color: var(--concert-blue)"
        >
          Place Order [{{ count }}] ({{ quantity }})
        </button>
      </nav>
      <div class="grid mb-8">
        <!-- START CARD -->
        <breakfast-menu
          v-for="kitchen in filterByName || filterByGroup"
          :key="kitchen.name"
          :name="kitchen.name"
          :image="kitchen.image"
          :groups="kitchen.groups"
          @addCart="cartHandler"
          :count="count"
          @removeCart="removeCartHandler"
        />
        <!-- END CARD -->
      </div>
      <nav
        class="
          flex flex-col
          md:flex-row
          bg-gray-300
          rounded
          shadow
          overflow-hidden
          p-2
          mb-8
          text-sm
        "
      >
        <label class="sr-only" for="search">Search</label>
        <input
          class="mb-2 md:mb-0 p-2 rounded flex-grow"
          type="search"
          placeholder="Search for foods"
          id="search"
        />
        <select class="mb-2 md:ml-2 md:mb-0" v-model="filterGroup">
          <option value="">Food Groups</option>
          <option v-for="group in uniqueQroups" :key="group" :value="group">
            {{ group }}
          </option>
        </select>
        <button
          class="md:ml-2 p-2 rounded"
          style="background-color: var(--concert-blue)"
        >
          Place Order [{{ count }}] ({{ quantity }})
        </button>
      </nav>
    </div>
  </div>
</template>

<script>
import BreakfastMenu from "./BreakfastMenu.vue";

export default {
  name: "BreakfastStudio",
  data() {
    return {
      query: "",
      filterGroup: "",
    };
  },
  props: ["count", "quantity"],
  inject: ["kitchens", "cartProvide"],
  methods: {
    cartHandler(name) {
      this.$emit("submit-cart", name);
    },
    removeCartHandler(name) {
      // console.log(name);
      this.$emit("submit-remove-cart", name);
    },
  },
  computed: {
    filterByName() {
      return this.kitchens.filter((kitchen) => {
        return (
          kitchen.name.toLowerCase().indexOf(this.query.toLowerCase()) >= 0
        );
      });
    },
    uniqueQroups() {
      const groups = [];
      this.kitchens.map((kitchen) => {
        kitchen.groups.map((group) => {
          groups.push(group);
        });
      });
      return [...new Set(groups)];
    },
    filterByGroup() {
      // console.log(this.filterGroup);
      return this.kitchens.filter((kitchen) => {
        return kitchen.groups.includes(this.filterGroup.toLowerCase());
      });
    },
  },
  components: {
    BreakfastMenu,
  },
  created() {
    // console.log(uniqueGroups);
  },
};
</script>


<style>
:root {
  --concert-blue: #5fdce3;
}

.grid {
  display: grid;
  grid-gap: 1em;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}

.concert-underline {
  display: inline-block;
  position: relative;
  z-index: 0;
}

.concert-underline::after {
  display: inline-block;
  content: "";
  height: 1em;
  width: 100%;
  background: var(--concert-blue);
  left: 0;
  bottom: 0.25em;
  position: absolute;
  transform: skew(45deg) scale(0.9);
  z-index: -1;
  opacity: 0.5;
}

.concert-underline::before {
  display: inline-block;
  content: "";
  height: 1em;
  width: 100%;
  background: yellow;
  top: 0;
  left: -0.25em;
  position: absolute;
  transform: skew(45deg) scale(0.9) translateX(-0.5em);
  z-index: -1;
  opacity: 0.5;
}
</style>
