<template>
  <div>
    <div class="flex items-center justify-between mb-10">
      <h1>Edit User</h1>
    </div>

    <el-form
      :model="form"
      ref="userForm"
      :rules="rules"
      label-position="top"
      class="flex flex-col"
    >
      <el-form-item label="First Name" prop="firstName">
        <el-input
          v-model="form.firstName"
          placeholder="Enter your first name"
        ></el-input>
      </el-form-item>
      <el-form-item label="Last Name" prop="lastName">
        <el-input
          v-model="form.lastName"
          placeholder="Enter your last name"
        ></el-input>
      </el-form-item>
      <el-form-item label="Age" prop="age">
        <el-input
          v-model.number="form.age"
          placeholder="Enter your age"
        ></el-input>
      </el-form-item>
      <el-form-item label="Phone" prop="phone">
        <el-input
          v-model="form.phone"
          placeholder="Enter your phone number"
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="handleSubmit">Save</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import api from "../../api/";
import { useRoute } from "vue-router";
import { ElNotification } from "element-plus";

const route = useRoute();
const userForm = ref(null);

const form = ref({
  firstName: "",
  lastName: "",
  age: null,
  phone: "",
});

const id = ref(route.params.id);

const getUser = () => {
  api
    .get(`/users/${id.value}`)
    .then((res) => {
      form.value = res.data;
    })
    .catch((err) => {
      ElNotification({
        title: "Error",
        message: err.message,
        type: "error",
      });
    });
};

onMounted(() => {
  getUser();
});

const rules = ref({
  firstName: [
    { required: true, message: "Please input first name", trigger: "change" },
    {
      min: 2,
      message: "First name must be at least 2 characters",
      trigger: "change",
    },
  ],
  lastName: [
    { required: true, message: "Please input last name", trigger: "change" },
    {
      min: 2,
      message: "Last name must be at least 2 characters",
      trigger: "change",
    },
  ],
  age: [
    { required: true, message: "Please input age", trigger: "change" },
    {
      type: "number",
      min: 0,
      message: "Age must be a non-negative number",
      trigger: "change",
    },
  ],
  phone: [
    { required: true, message: "Please input phone number", trigger: "change" },
    {
      message: "Phone number must be 10 digits",
      trigger: "change",
    },
  ],
});

const handleSubmit = () => {
  userForm.value.validate((valid) => {
    if (valid) {
      api
        .put(`/users/${id.value}`)
        .then(() => {
          ElNotification({
            title: "Successfully updated user",
            type: "success",
          });
        })
        .catch((err) => {
          ElNotification({
            title: "Error",
            message: err.message,
            type: "error",
          });
        });
    } else {
      ElNotification({
        title: "All data is requirred!",
        type: "error",
      });
    }
  });
};
</script>

<style scoped></style>
