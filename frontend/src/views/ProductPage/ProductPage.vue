<template>
  <section class="product__page">
    <div class="container">
      <div class="product__inner">
        <div class="product__info">
          <img :src="product().image" alt="" />
          <div class="product__content">
            <label
              ><h2>{{ product().name }}</h2></label
            >
            <label
              ><p>{{ product().description }}</p></label
            >
            <ul>
              <li>Дата: {{ product().date.slice(0,10) }}</li>
              <li>Время: {{ product().time.toString().slice(0,5)}}</li>
              <li>Адрес: {{ product().address }}</li>
            </ul>
            <div class="cart_info">
              <label
                >Стоимость
                <h2 class="price">
                  {{ " " + product().price + " " + "₽" }}
                </h2></label
              >
              <label 
              v-if = "ISAUTHORIZED" >Количество:
                <input
                  type="number"
                  min="1"
                  class="quantity"
                  v-model="quantity"
                  @input = "validate($event)"
                 
              /></label>
              <button @click="addToCart" class="button" v-if="ISAUTHORIZED">
                Добавить в корзину
              </button>
              <span v-if="!ISAUTHORIZED" class="please_login">Зарегистрируйтесь, чтобы добавить товар в корзину</span>
            </div>
          </div>
        </div>
        
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";
import { mapGetters } from "vuex";
import { mapMutations } from "vuex";

export default {
  props: {
    id: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      products: [],
      quantity: 1,
    };
  },
  methods: {
    ...mapMutations(["setCartSize"]),
    async getProducts() {
      const response = await axios.get("http://localhost:3000/products");
      this.products = response.data;
    },
    product() {
      const result = this.products.find((product) => product.id == this.id);
      return result;
    },
    async addToCart() {
      const body = {
        user_id: this.USERID,
        cart_id: this.USERID,
        product_id: this.id,
        quantity: this.quantity,
      };
      try {
        const response = await axios.post("http://localhost:3000/cart", body);
        this.updateTotalQuantity();
        alert(response.data.message);
      } catch (error) {
        if (
          error.response &&
          error.response.data &&
          error.response.data.error
        ) {
          alert(error.response.data.error);
        } else {
          alert("Ошибка");
        }
      }
    },
    async updateTotalQuantity() {
      const userId = this.USERID;
      const response = await axios.get(`http://localhost:3000/cart/${userId}`);
      const total = response.data.reduce((acc, item) => {
        return acc + item.product_quantity;
      }, 0);
      this.setCartSize(total);
    },
    validate($event) {
      if ($event.target.value < 1) {
        $event.target.value = 1;
      }
    },
  },
  computed: {
    ...mapGetters(["USERID", "ISAUTHORIZED"]),
  },
  beforeMount() {
    this.getProducts();
  },
};
</script>
