<template>
    <div class="page">
      <h4>Thêm Liên hệ Mới</h4>
      <ContactForm @submit:contact="addContact" />
      <p>{{ message }}</p>
    </div>
  </template>
  
  <script>
  import ContactForm from "@/components/ContactForm.vue";
  import ContactService from "@/services/contact.service";
  
  export default {
    components: {
      ContactForm,
    },
    data() {
      return {
        message: "",
      };
    },
    methods: {
      async addContact(data) {
        try {
          await ContactService.create(data); // Gọi API để thêm liên hệ
          this.message = "Liên hệ đã được thêm thành công!";
          this.$router.push({ name: "contactbook" }); // Chuyển về trang danh bạ
        } catch (error) {
          console.error(error);
          this.message = "Đã có lỗi xảy ra khi thêm liên hệ.";
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .page {
    max-width: 600px;
    margin: auto;
  }
  </style>