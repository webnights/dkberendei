<template>
  <section class="contacts">
    <div class="container">
      <div class="contacts__inner">
        <h2 class="title">Контакты</h2>
        <ul>
          <ContactItem
            v-for="(contact, index) in contacts"
            :key="index"
            :contact="contact"
          />
        </ul>
        <div class="contacts__map">
          <iframe
            src="https://yandex.ru/map-widget/v1/?um=constructor%3Af82444967e4186b3c735c1f01f1a770a56e2f08a693ce40d223f03698a93f7bb&amp;source=constructor"
            width="966"
            height="314"
            frameborder="0"
          ></iframe>
        </div>
      </div>
    </div>
  </section>
</template>
<script>
import axios from "axios";
import ContactItem from "/src/components/ContactItem/ContactItem.vue";
export default {
  components: {
    ContactItem,
  },
  data() {
    return {
      contacts: [],
    };
  },
  methods: {
    async getContacts() {
      try {
        const response = await axios.get("http://localhost:3000/contacts");
        this.contacts = response.data;
      } catch (error) {
        console.log(
          error,
          "Произошла ошибка при взятии данных из таблицы Контакты"
        );
      }
    },
  },
  mounted() {
    this.getContacts();
  },
};
</script>
