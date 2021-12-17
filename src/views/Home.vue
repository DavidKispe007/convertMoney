<template>
  <div class="home">
    <div class="home-welcome">
      <!-- <pre> {{ values }} </pre>
      <pre> {{ typeCoin }} </pre> -->
      <div>
        <h1>
          El mejor <br />
          tipo de cambio
        </h1>
        <h3>para cambiar dólares y soles online en Perú</h3>
      </div>
    </div>

    <div class="home-box">
      <div class="wrapper">
        <price-today
          :typeCoin="typeCoin"
          :values="values"
          @handleSelectAction="handleSelectAction"
          @changeType="changeType"
        />
        <calculator
          :typeCoin="typeCoin"
          :moneyConverter="form"
          @changeType="changeType"
          @handledConverterMoney="handledConverterMoney"
        />

        <div class="wrapper-btn-start">
          <button class="btn-primary-100">Iniciar operación</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import priceToday from "../components/converter/priceDollarTodayShow";
import calculator from "../components/converter/calculator";

export default {
  name: "Home",
  components: {
    priceToday,
    calculator,
  },

  data() {
    return {
      values: {},
      interval: null,
      plusOne: 1,
      inCase: null,

      // dinamic States
      typeCoin: 0,
      form: {
        sent: 10,
        received: null,
      },
    };
  },

  mounted() {
    this.loadData();
    this.interval = setInterval(
      function () {
        this.plusOne += 1;
        this.loadData();
      }.bind(this),
      60000
    );
  },

  methods: {
    loadData() {
      fetch(`https://exchangeratecs.herokuapp.com/api/rates`)
        .then((response) => response.json())
        .then((json) => {
          this.values = json;
          this.inCase = 1;
          this.form.received = (
            this.form.sent * this.values.purchase_price
          ).toFixed(2);
        });
    },

    // HEADER
    handleSelectAction(val) {
      this.typeCoin = val;
    },

    //BODY
    handledConverterMoney(data) {
      this.convertidor(data);
    },

    toFixed2(value) {
      return value.toFixed(2);
    },

    changeType(val) {
      val == 0 ? (this.typeCoin = 1) : (this.typeCoin = 0);
      if (this.typeCoin == 0) {
        this.form.received = this.toFixed2(
          this.form.sent * this.values.purchase_price
        );
      } else {
        this.form.received = this.toFixed2(
          this.form.sent / this.values.sale_price
        );
      }
    },

    convertidor(data) {
      if (data.n == 1 && this.typeCoin == 0) {
        this.form.received = this.toFixed2(data.form.sent * this.values.purchase_price);
      }

      if (data.n == 1 && this.typeCoin == 1) {
        this.form.received = this.toFixed2(data.form.sent / this.values.sale_price);
      }

      if (data.n == 2 && this.typeCoin == 0) {
        this.form.sent = this.toFixed2(data.form.received / this.values.purchase_price);
      }

      if (data.n == 2 && this.typeCoin == 1) {
        this.form.sent = this.toFixed2(data.form.received * this.values.sale_price);
      }
    },
  },

  beforeDestroy() {
    clearInterval(this.interval);
  },
};
</script>

<style lang="scss">
@import "./homeStyle.scss";
</style>
