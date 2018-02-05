<template>
  <section class="cart">
    <div class="cart-item" v-for="cartItem in widgetCartItems">
      <div class="heading">
        <div class="name">
          <h3>{{cartItem.state.name}} {{cartItem.state.price.price | currency}}</h3>
        </div>
        <div class="buttons">
          <button @click="removeItem(cartItem.id)">X</button>
        </div>
      </div>
      <div class="main-options">
        <div class="option">
          <label>
            <span>Billing cycles count</span>
            <select class="billing-cycles-count" :value="cartItem.state.billingCyclesCount" @change="billingCyclesCountChange(cartItem.id, $event)">
              <option value="1">1</option>
              <option value="2">2</option>
              <option value="3">3</option>
              <option value="4">4</option>
              <option value="5">5</option>
            </select>
          </label>
        </div>
        <div class="option">
          <label>
            <span>Auto-renew</span>
            <input type="checkbox" :checked="cartItem.state.autorenew" @change="autorenewChange(cartItem.id, $event)"/>
          </label>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
  export default {
    name: 'Default',
    props: {
      widgetSdk: {
        type: Object,
        required: true,
      },
      items: {
        type: Array,
        default: [],
      },
    },
    data() {
      return {
        widgetCartItems: this.items
      }
    },
    created() {
      this.widgetSdk.subscribe(this.listener);
    },
    methods: {
      autorenewChange(id, e) {
        this.widgetSdk.setAutorenew(id, e.target.checked);
      },
      billingCyclesCountChange(id, e) {
        this.widgetSdk.setBillingCyclesCount(id, e.target.value);
      },
      // eslint-disable-next-line no-unused-vars
      listener({actionType, payload, error}) {
        if (error) {
          return;
        }

        this.widgetCartItems = payload;

      },
      removeItem(id) {
        this.widgetSdk.removeItem(id);
      }
    }
  }
</script>

<style scoped>
  .cart {
    width: 768px;
    border: 1px solid #F5F5F5;
    border-radius: 10px;
    background-color: #F5F5F5;
    padding: 15px;
    margin: 20px;
  }

  .cart-item {
    position: relative;
    margin: 0 auto;
    border: 1px solid #EEEEEE;
    border-radius: 10px;
    background-color: #FFFFFF;
    padding: 25px;
  }

  .cart-item + .cart-item {
    margin-top: 15px;
  }

  .cart-item .heading {
    display: flex;
    margin: 0 0 15px;
  }

  .cart-item .name {
    flex: 1;
  }

  .cart-item h3 {
    margin: 0;
  }

  .cart-item .main-options {
    display: flex;
  }

  .cart-item .option {
    padding: 15px;
    border: 1px solid #ccc;
  }

  .cart-item .option + .option {
    margin: 0 0 0 15px;
  }

  .cart-item .option label > span {
    margin: 0 5px 0 0;
  }

  .cart-item select {
    min-width: 100px;
  }
</style>
