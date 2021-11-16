<template>
  <breakfast-studio
    @submitCart="cart"
    :count="countOfMenuCart"
    :quantity="quantityOfMenu"
    @submitRemoveCart="removeCart"
  />
</template>

<script>
import BreakfastStudio from "./components/BreakfastStudio.vue";
import DUMMY_DATA from "./kitchen";
export default {
  name: "App",
  data() {
    return {
      kitchens: DUMMY_DATA,
      cartStore: [],
    };
  },
  methods: {
    cart(data) {
      // console.log(data);
      let quantity = 0;
      const res = this.kitchens.find(
        ({ name }) => name.toLowerCase() === data.toLowerCase()
      );
      quantity += 1;
      const obj = { ...res, quantity };
      // console.log(obj);
      if (this.cartStore.length <= 0) {
        this.cartStore.push(obj);
      } else if (this.cartStore.length > 0) {
        const findIndexCart = this.cartStore.findIndex(
          (cart) => cart.name == obj.name
        );

        // console.log(findIndexCart);

        if (findIndexCart !== -1) {
          quantity = this.cartStore[findIndexCart].quantity += 1;
          this.cartStore[findIndexCart] = { ...obj, quantity };
        } else {
          quantity = 0;
          quantity += 1;
          this.cartStore[this.cartStore.length] = { ...obj, quantity };
        }
      }
    },
    removeCart(data) {
      // console.log(data.toLowerCase());
      // console.log(this.cartStore);
      // const menu = this.cartStore.find(({ name }) => {
      //   name.toLowerCase() === data.toLowerCase();
      // });
      const findIndexCart = this.cartStore.findIndex(
        (cart) => cart.name.toLowerCase() == data.toLowerCase()
      );
      if (this.cartStore[findIndexCart] !== -1) {
        this.cartStore[findIndexCart].quantity -= 1;
      }

      if (this.cartStore[findIndexCart].quantity === 0) {
        this.cartStore.splice(findIndexCart, 1);
      }
    },
  },
  computed: {
    countOfMenuCart() {
      return this.cartStore.length;
    },
    quantityOfMenu() {
      let total = 0;
      if (this.cartStore.length > 0) {
        total = this.cartStore.reduce((prevState, curretState) => {
          return prevState + curretState.quantity;
        }, 0);
      }
      return total;
    },
  },
  provide() {
    return {
      kitchens: this.kitchens,
      cartProvide: this.cartStore,
    };
  },
  components: {
    BreakfastStudio,
  },
  updated() {
    // console.log(this.cartStore);
  },
};
</script>

<style>
</style>
