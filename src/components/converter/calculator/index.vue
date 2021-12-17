<template>
  <div class="converter">
    <form @submit.prevent="handleCalculate">
      <div class="converter-input">
        <span>{{ nameMoney(0) }}</span>
        <div class="div-wrapper">
          <small>Envías</small>
          <input
            type="number"
            v-model.number="form.sent"
            placeholder=""
            onkeydown="return  event.keyCode !== 109 && event.keyCode !== 69 && event.keyCode !== 107"
            @keyup="handledConverterMoney(nameMoney(0), 1)"
          />
        </div>
      </div>

      <div class="btn-changer">
        <span class="btn-changer-span" @click="changeType">
          <img src="../../../assets/img/icoChanger.png" alt="" />
        </span>
      </div>

      <div class="converter-input">
        <span>{{ nameMoney(1) }}</span>
        <div class="div-wrapper">
          <small>Recibes</small>
          <input
            type="number"
            v-model.number="form.received"
            placeholder=""
            onkeydown="return  event.keyCode !== 109 && event.keyCode !== 69 && event.keyCode !== 107"
            @keyup="handledConverterMoney(nameMoney(1), 2)"
          />
        </div>
      </div>

      <!-- <button>Convertir</button> -->
    </form>

    <!-- <pre> ::: {{ form }} </pre> -->
    <pre>{{ printMoney }}</pre>
  </div>
</template>

<script>
export default {
  props: {
    typeCoin: {
      type: Number,
      default: 0,
    },
    moneyConverter: {
      type: Object,
      default: {},
    },
  },
  data() {
    return {
      separator: "---",
      form: {
        sent: null,
        received: null,
      },
    };
  },

  computed: {
    printMoney() {
      this.form = this.moneyConverter;
    },
  },

  methods: {
    nameMoney(val) {
      return this.typeCoin == val ? "Dólares" : "Soles";
    },

    changeType() {
      this.$emit("changeType", this.typeCoin);
    },

    handledConverterMoney(v, n, separator) {
      //

      // console.log("KKKKKK", v, n);

      this.$emit("handledConverterMoney", { form: this.form, v, n });
    },
  },
};
</script>

<style lang="scss">
@import "@/scss/globalStyle.scss";
.converter {
  padding: 2rem 2rem 1rem 2rem;
  &-input {
    display: flex;
    justify-content: space-between;
    border: 1px solid $main-color;
    height: 48px;
    border-radius: 5px;
    // padding: 0.1rem;
    span {
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 14px;
      color: $main-color;
      flex: 1.5;
      background-color: #f3f3f6;
      border-radius: 5px 0 0 5px;
      position: relative;
      z-index: 2;
      font-weight: 400;
    }

    .div-wrapper {
      flex: 2;
      display: flex;
      flex-direction: column;
      align-items: flex-end;
      justify-content: center;
      small {
        padding-right: 1rem;
        color: $color-small;
        font-weight: 400;
      }
    }
    input {
      color: $color-number;
      border: none;
      text-align: right;
      width: 100%;
      padding: 0 1rem;
      font-size: 18px;
      &:focus {
        outline: none;
      }
    }
  }
}

.btn-changer {
  // outline: 1px solid red;
  position: relative;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9;

  // top: 50%;
  // left: 50%;
  &-span {
    cursor: pointer;
    position: absolute;
    background-color: $main-color;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 100%;
  }
}
</style>
