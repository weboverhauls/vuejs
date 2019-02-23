<template>
  <details ref="details"
    ><summary data-vue-menu-button>Shopping Cart</summary>
    <div data-vue-menu>Content</div>
  </details>
</template>

<script>
export default {
  created() {
    document.addEventListener("click", this.documentClick);
  },
  methods: {
    documentClick(e) {
      let el = this.$refs.details;
      let target = e.target;
      if (el !== target && !el.contains(target)) {
        this.closeHandler();
      }
    },
    closeHandler() {
      this.$refs.details.removeAttribute("open");
    }
  }
};
</script>

<style scoped>
[open] [data-vue-menu] {
  background-color: #ffffff;
  list-style: none;
  margin: 10px 0 0 0;
  width: 220px;
  padding: 10px 0;
  border: 1px solid #2368a2;
  animation: slide-down 0.2s ease;
  position: absolute;
  z-index: 10;
  box-shadow: 4px 4px 6px 0 #6665654d;
}

[data-vue-menu-button] {
  list-style: none;
}

[data-vue-menu]::before {
  content: "";
  width: 0;
  height: 0;
  position: absolute;
  border-left: 7px solid transparent;
  border-right: 7px solid transparent;
  border-bottom: 7px solid #2368a2;
  top: -7px;
  left: 15px;
}

[data-vue-menu]::after {
  content: "";
  width: 0;
  height: 0;
  position: absolute;
  border-left: 6px solid transparent;
  border-right: 6px solid transparent;
  border-bottom: 6px solid white;
  top: -6px;
  left: 16px;
}

[data-vue-menu-item],
[data-vue-menu-link] {
  color: inherit;
  text-decoration: none;
  line-height: 28px;
  height: 28px;
  cursor: pointer;
  padding: 0 10px;
  display: block;
}

[data-vue-menu-item]:focus,
[data-vue-menu-link]:focus {
  outline: 0;
  background-color: #2368a2;
  color: #fff;
  display: block;
}

[data-vue-menu-item]:hover,
[data-vue-menu-link]:hover {
  outline: 0;
  background-color: #2368a2;
  color: #fff;
  display: block;
}

[tabindex="-1"]:focus,
*::-moz-focus-inner {
  border: 0;
}

@keyframes slide-down {
  0% {
    opacity: 0;
    transform: translateY(-10px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>