<template>
  <fieldset>
    <div class="store">
      <!-- <h2> {{ storeTitle }} </h2> -->
      <div class="items" v-for="item of items" :key="item.id">
        <h3>{{ item.name }}</h3>
        <img :src="item.imageUrl" />  
        <h4>{{ item.description }}</h4>
        <button @click="removeFromCart(item.id)" v-if="isInCart(item.id)">
            {{ removeBtn }}
        </button>
        <button @click="addToCart(item.id)" v-else>add to cart</button>
        <button @click="$router.push('/cart')">check out</button>
      </div>
    </div>
    </fieldset>
</template>

<script>
const items = Object.freeze([
  {
    id: 1,
    // imageUrl: "https://tinyurl.com/zbw8pyv4",
    imageUrl: "https://tinyurl.com/4wr9szbn",
    name: "One Sauce. No Options.",
    description: "Mama Georgio will die of a broken heart if you do not eat her sauce.",
    // description: "Mama Georgio's"
  },
  {
    id: 2,
    // imageUrl: "https://tinyurl.com/6xhbfa7k",
    imageUrl: "https://tinyurl.com/a3nd866d",
    name: "The Only Noodles.",
    description:"Mama Georgio will turn off her hearing aid if you ask if these noodles are gluten-free.",
  },
  // {
  //   id: 3,
  //   imageUrl: "https://tinyurl.com/4wr9szbn",
  //   name: "Meat.. If That's Your Thing",
  //   description: "Mama Georgio highly recommends meat to accompany her sauce. She will respect your choice to abstain from meat. However, she will cross herself while muttering in old-lady Italian and will be highly suspiscious of you from that point on.",
  // },
]);

export default {
  name: "Store",
  data() {
    return {
      // storeTitle: "Store",
      items,
      cart: [],
      removeBtn: "remove from cart",
    };
  },
  methods: {
    isInCart(itemId) {
      if (!localStorage.getItem("cart")) {
        localStorage.setItem("cart", JSON.stringify([]));
      }
      const cartItem = this.cart.find(({ id }) => id === itemId);
      return Boolean(cartItem);
    },
    addToCart(itemId) {
      const item = this.items.find(({ id }) => id === itemId);
      if (!localStorage.getItem("cart")) {
        localStorage.setItem("cart", JSON.stringify([]));
      }
      const cartItems = JSON.parse(localStorage.getItem("cart"));
      cartItems.push(item);
      localStorage.setItem("cart", JSON.stringify(cartItems));
      this.cart = JSON.parse(localStorage.getItem("cart"));
    },
    removeFromCart(itemId) {
      const cartItems = JSON.parse(localStorage.getItem("cart"));
      const index = cartItems.findIndex(({ id }) => id === itemId);
      cartItems.splice(index, 1);
      localStorage.setItem("cart", JSON.stringify(cartItems));
      this.cart = JSON.parse(localStorage.getItem("cart"));
    },
  },
};
</script>

<style lang="scss">
  @import "../scss/variables.scss";
  @import "../scss/mixins.scss";
  fieldset {
    // Styles carried over from App page
    // @include fieldsetStyle();
    .store {
      @include storeCart();
      h2, h3, h4 {
        @include defaultText();
      }
      h2 {
        display: flex;
        flex-direction: row;
        justify-content: center;
        font-weight:900;
      }
      .items {
        padding:16px 8px;
        h3 {
          max-width: 400px;
          margin: $size1 auto;
          text-align: center;
        }
        img {
          width:100%;
          max-width:400px;
          height:auto;
          border-radius:$size1;
          margin:0 auto;
          display: flex;
        }
        h4 {
          display: flex;
          flex-direction:row;
          justify-content: center;
          padding:0 12px;
          margin-top: 8px;
          font-style:italic;
          font-weight: 900;
        }
        button {
          @include styleRemoval();
          @include buttonStyle();
          width:45%;
          height:$size6;
          margin: 0px 10px;
          &:hover {
            cursor:pointer;
            background-color: lighten($background, 6%);
            transition: all 0.65s ease;
          }
        }
      }
      // .items:nth-child(3) > img {
        // transform: rotate(90deg);
        // max-height:400px;
        // height:100%;
        // width:auto;
        // max-width: 100%;
      // }
    }
  }
  // X-Small devices (portrait phones, less than 576px)
  @media (max-width: 575.98px) {
    fieldset {
      .store {
        max-width:250px;
        padding:0 30px 15px;
        .items {
          padding:15px 0;
          max-width:250px;
          h3 {
            max-width:300px;
            margin: 8px auto 16px;
          }
          img {
            max-width:250px;
          }
          h4 {
            padding:0;
            text-align:center;
            font-size:15px;
          }
          button {
            width:auto;
            max-width:100%;
            min-width:250px;
            margin:0 auto 10px;
            display:flex;
          }
        }
      }
    }
  }
</style>