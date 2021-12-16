<template>
  <div class="home">
    <div class="home-welcome">
      <div>
        <h1>El mejor <br> tipo de cambio</h1>
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
          this.inCase = 1
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

    changeType(val) {
      val == 0 ? (this.typeCoin = 1) : (this.typeCoin = 0);

      if (this.inCase == 1 || this.inCase == 2) {
        if (this.typeCoin == 1) {
          this.form.received = (this.form.sent / this.values.sale_price).toFixed(2);
        } else {
          this.form.received = (this.form.sent * this.values.purchase_price).toFixed(2);
        }
      } else if (this.inCase == 3 || this.inCase == 4) {
        if (this.typeCoin == 1) {
          this.form.sent = (this.form.received * this.values.sale_price).toFixed(2);
        } else {
          this.form.sent = (this.form.received / this.values.purchase_price).toFixed(2);
        }
      }
    },

    convertidor(data) {

      // if(data.n == 1) {
      //   let parts = this.form.sent.toString().split('.');
      //   parts[0] = parts[0].toString().replace(/\D*/g, '').replace(/\B(?=(\d{3})+(?!\d))/g); 
      //   this.form.sent = '$ ' + parts.join('.');
      //   return this.form.sent
      // } else if(data.n == 2) {
      //   let parts = this.form.received.toString().split('.');
      //   parts[0] = parts[0].toString().replace(/\D*/g, '').replace(/\B(?=(\d{3})+(?!\d))/g); 
      //   this.form.received = '$ ' + parts.join('.');
      //   return this.form.received
      // }

      if (data.v == "Dólares" && data.n == 1) {
        if (this.typeCoin == 0) {
          this.inCase = 1;
          this.form.received = (
            this.form.sent * this.values.purchase_price
          ).toFixed(2);
        }
      }

      if (data.v == "Soles" && data.n == 1) {
        if (this.typeCoin == 1) {
          this.inCase = 2;
          this.form.received = (
            this.form.sent / this.values.sale_price
          ).toFixed(2);
        }
      }

      if (data.v == "Dólares" && data.n == 2) {
        if (this.typeCoin == 1) {
          this.inCase = 3;
          this.form.sent = (
            this.form.received * this.values.sale_price
          ).toFixed(2);
        }
      }

      if (data.v == "Soles" && data.n == 2) {
        if (this.typeCoin == 0) {
          this.inCase = 4;
          this.form.sent = (
            this.form.received / this.values.purchase_price
          ).toFixed(2);
        }
      }
    },
  },

  created() {
    // let data = {
    //   form: {
    //     received: 10,
    //     sent: null,
    //   },
    //   n: 1,
    //   v: "Dólares",
    // };
    // this.convertidor(data);
  },

  beforeDestroy() {
    clearInterval(this.interval);
  },
};
</script>

<style lang="scss">
@import "./homeStyle.scss";
</style>
