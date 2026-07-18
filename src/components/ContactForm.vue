
<template>
  <Form @submit="submitContact" :validation-schema="contactFormSchema">
    <!-- Tên -->
    <div class="form-group">
      <label for="name">Tên</label>

      <Field
        name="name"
        type="text"
        class="form-control"
        v-model="contactLocal.name"
      />

      <ErrorMessage name="name" class="error-feedback" />
    </div>

    <!-- Email -->
    <div class="form-group">
      <label for="email">E-mail</label>

      <Field
        name="email"
        type="email"
        class="form-control"
        v-model="contactLocal.email"
      />

      <ErrorMessage name="email" class="error-feedback" />
    </div>

    <!-- Địa chỉ -->
    <div class="form-group">
      <label for="address">Địa chỉ</label>

      <Field
        name="address"
        type="text"
        class="form-control"
        v-model="contactLocal.address"
      />

      <ErrorMessage name="address" class="error-feedback" />
    </div>

    <!-- Điện thoại -->
    <div class="form-group">
      <label for="phone">Điện thoại</label>

      <Field
        name="phone"
        type="tel"
        class="form-control"
        v-model="contactLocal.phone"
      />

      <ErrorMessage name="phone" class="error-feedback" />
    </div>

    <!-- Yêu thích -->
    <div class="form-group form-check">
      <input
        type="checkbox"
        class="form-check-input"
        id="favorite"
        v-model="contactLocal.favorite"
      />

      <label class="form-check-label" for="favorite">
        <strong>Liên hệ yêu thích</strong>
      </label>
    </div>

    <!-- Sở thích -->
    <div class="form-group">
      <label><strong>Sở thích</strong></label>

      <div
        class="form-check"
        v-for="hobby in availableHobbies"
        :key="hobby"
      >
        <input
          class="form-check-input"
          type="checkbox"
          :id="hobby"
          :value="hobby"
          v-model="contactLocal.hobbies"
        />

        <label
          class="form-check-label"
          :for="hobby"
        >
          {{ hobby }}
        </label>
      </div>
    </div>

    <!-- Button -->
    <div class="form-group mt-3">
      <button class="btn btn-primary">
        Lưu
      </button>

      <button
        v-if="contactLocal._id"
        type="button"
        class="btn btn-danger ml-2"
        @click="deleteContact"
      >
        Xóa
      </button>

      <button
        type="button"
        class="btn btn-secondary ml-2"
        @click="Cancel"
      >
        Thoát
      </button>
    </div>
  </Form>
</template>

<script>
import { ErrorMessage, Field, Form } from "vee-validate";
import * as yup from "yup";

export default {
  components: {
    Form,
    Field,
    ErrorMessage,
  },

  props: {
    contact: {
      type: Object,
      required: true,
    },
  },

  emits: ["submit:contact", "delete:contact"],

  data() {
    const contactFormSchema = yup.object({
      name: yup
        .string()
        .required("Tên phải có giá trị.")
        .min(2, "Tên ít nhất 2 ký tự.")
        .max(50, "Tên tối đa 50 ký tự."),

      email: yup
        .string()
        .email("Email không hợp lệ.")
        .max(50, "Email tối đa 50 ký tự."),

      address: yup
        .string()
        .max(100, "Địa chỉ tối đa 100 ký tự."),

      phone: yup
        .string()
        .matches(
          /(09|03|07|08|05)+([0-9]{8})\b/,
          "Số điện thoại không hợp lệ."
        ),
    });

    return {
      contactLocal: {
        ...this.contact,
        hobbies: Array.isArray(this.contact.hobbies)
          ? [...this.contact.hobbies]
          : [],
      },

      availableHobbies: [
        "Đá bóng",
        "Bóng chuyền",
        "Câu cá",
        "Ca hát",
        "Chơi đàn",
        "Chạy bộ",
      ],

      contactFormSchema,
    };
  },

  methods: {
    submitContact() {
      this.$emit("submit:contact", this.contactLocal);
    },

    deleteContact() {
      this.$emit("delete:contact", this.contactLocal._id);
    },

    Cancel() {
      const reply = window.confirm(
        "Bạn có thay đổi chưa lưu. Bạn có muốn thoát không?"
      );

      if (reply) {
        this.$router.push({ name: "contactbook" });
      }
    },
  },
};
</script>

<style scoped>
@import "@/assets/form.css";
</style>
