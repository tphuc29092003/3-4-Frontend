<template>
    <div v-if="contact" class="page">
      <h4>Hiệu chỉnh Liên hệ</h4>
      <ContactForm
        :contact="contact"
        @submit:contact="updateContact"
        @delete:contact="deleteContact"
      />
      <p v-if="message" class="message">{{ message }}</p>
    </div>
  </template>
  
  <script>
  import ContactForm from "@/components/ContactForm.vue";
  import ContactService from "@/services/contact.service";
  
  export default {
    components: {
      ContactForm,
    },
    props: {
      id: { type: String, required: true },
    },
    data() {
      return {
        contact: null,
        message: "",
      };
    },
    methods: {
      async getContact(id) {
        try {
          this.contact = await ContactService.get(id);
        } catch (error) {
          console.error("Error fetching contact:", error);
          // Chuyển đến trang NotFound nếu không tìm thấy
          this.$router.push({
            name: "notfound",
            params: {
              pathMatch: this.$route.path.split("/").slice(1),
            },
            query: this.$route.query,
            hash: this.$route.hash,
          });
        }
      },
      async updateContact(data) {
        try {
          await ContactService.update(this.contact._id, data);
          this.message = 'Liên hệ được cập nhật thành công.';
          setTimeout(() => {
            this.message = ""; // Xóa thông báo sau 3 giây
          }, 3000);
          this.$router.push({ name: "contactbook" });
        } catch (error) {
          console.error("Error updating contact:", error);
          this.message = "Cập nhật không thành công. Vui lòng thử lại.";
        }
      },
      async deleteContact() {
        if (confirm("Bạn muốn xóa Liên hệ này?")) {
          try {
            await ContactService.delete(this.contact._id);
            this.$router.push({ name: "contactbook" });
          } catch (error) {
            console.error("Error deleting contact:", error);
            this.message = "Xóa không thành công. Vui lòng thử lại.";
          }
        }
      },
    },
    created() {
      this.getContact(this.id);
      this.message = "";
    },
  };
  </script>
  
  <style scoped>
  .message {
    color: green; /* Hoặc màu đỏ cho thông báo lỗi */
    margin-top: 10px;
  }
  </style>
  