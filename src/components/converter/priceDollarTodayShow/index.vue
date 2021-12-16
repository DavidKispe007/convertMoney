<template>
  <div class="price_dolar" v-if="values">
    <div
      class="price_dolar-wrapper"
      :class="activeClass(0)"
      @click="handleSelectAction(0)"
    >
      <span>Dólar compra</span>
      <span>{{ values.purchase_price }}</span>
    </div>

    <div
      class="price_dolar-wrapper"
      :class="activeClass(1)"
      @click="handleSelectAction(1)"
    >
      <span>Dólar venta</span>
      <span>{{ values.sale_price }}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    typeCoin: {
      type: Number,
      default: 0,
    },
    values: {
      type: Object,
      default: {},
    },
  },

  methods: {
    handleSelectAction(val) {
      this.$emit("handleSelectAction", val);
      this.changeType()
    },
    changeType() {
      this.$emit("changeType", this.typeCoin);
    },
    activeClass(val) {
      return this.typeCoin == val ? "active" : "";
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../../../scss/globalStyle.scss";
.price_dolar {
  display: flex;
  justify-content: center;
    border-bottom: 1px solid #f3f3f6;
    padding: 1.5rem 1rem 0 1rem;

  &-wrapper {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    font-size: 14px;
    max-width: 90px;
    margin: 0 1rem;
    cursor: pointer;
    color: $color-small;
    span:nth-child(1) {
      font-weight: 400;
    }
    span:nth-child(2) {
      font-weight: 500;
    }
  }

  .active {
    color: $main-color;
    position: relative;
    &:after {
      content: " ";
      position: absolute;
      background-color: $main-color;
      height: 2px;
      width: 100%;
      bottom: -2px;
      z-index: 99;
    }
  }
}
</style>