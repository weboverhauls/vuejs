<template>
  <navigation-menu class="c-shopping-cart" ref="details">
    <template slot="head">
      <transition name="bounce">
        <div
          class="shopping-cart__item-count"
          :key="numberOfCartItems"
          v-show="numberOfCartItems"
        >
          {{ numberOfCartItems }}
        </div>
      </transition>
    </template>
    <template slot="summary"
      >Shopping Cart
    </template>
    <template slot="details-content">
      <div data-vue-menu>
        <div v-if="shoppingCartItems.length">
          <ul
            class="c-shopping-cart__list"
            :key="item.id"
            v-for="item in shoppingCartItems"
          >
            <li class="c-shopping-cart__list-item">
              {{ item.title }}
              <button
                data-close-button
                @click="removeItem(item)"
                aria-label="Remove product from shopping cart"
                class="o-button o-button--bare"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="square"
                  stroke-linejoin="arcs"
                >
                  <polyline points="3 6 5 6 21 6"></polyline>
                  <path
                    d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"
                  ></path>
                  <line x1="10" y1="11" x2="10" y2="17"></line>
                  <line x1="14" y1="11" x2="14" y2="17"></line>
                </svg>
              </button>
            </li>
          </ul>
          <button
            @click="removeAllItems"
            class="u-button-link-look o-link c-shopping-cart__remove-all"
          >
            Remove all items
          </button>
        </div>
        <div v-if="!shoppingCartItems.length">
          Your shopping cart is empty <span aria-hidden>:(</span>
        </div>
      </div>
    </template>
  </navigation-menu>
</template>

<script>
import store from "../store";
import NavigationMenu from "./NavigationMenu";

export default {
  components: {
    NavigationMenu
  },
  created() {
    document.addEventListener("click", this.documentClick);
  },
  computed: {
    shoppingCartItems: function() {
      return store.getters.shoppingCartItems;
    },
    numberOfCartItems: function() {
      return this.shoppingCartItems.length;
    }
  },
  methods: {
    removeItem(item) {
      let detailEl = this.$refs.details.$el.children[1];

      store.commit("toggleShoppingCartState", item);

      setTimeout(() => {
        // Only here query for data-close-buttons
        let closeButtons = detailEl.querySelectorAll(
          "[data-close-button]"
        );

        if (closeButtons.length) {
          closeButtons[0].focus();
        }

        let message = `${item.title} has been removed`;
        this.$announcer.set(message);
      });
    },
    removeAllItems() {
      store.commit("removeAllShoppingCartItems");

      let message = `Shopping cart is now empty`;
      this.$announcer.set(message);
    }
  }
};
</script>

<style scoped>
.c-shopping-cart[data-vue-menu] {
  padding: 10px;
}

.c-shopping-cart__list {
  list-style: none;
  margin: 0;
  padding: 0;
}

.c-shopping-cart {
  position: relative;
}

.shopping-cart__item-count {
  position: absolute;
  right: -0.5rem;
  top: -0.5rem;
  width: 1.25rem;
  height: 1.25rem;
  background-color: #484848;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  border: 1px solid;
  color: #fff;
  font-weight: 700;
  font-size: 80%;
}

.c-shopping-cart__list-item {
  display: flex;
  justify-content: space-between;
  margin: 0.75rem 0;
}

.c-shopping-cart__remove-all {
  margin-top: 0.66rem;
  float: right;
}

.bounce-enter-active {
  animation: bounce-in 0.5s;
}

@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.25);
  }
  100% {
    transform: scale(1);
  }
}
</style>
