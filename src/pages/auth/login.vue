<template>
  <div class="flex justify-center items-center h-screen bg-gray-100">
    <div class="w-full max-w-md p-8 space-y-6 bg-white rounded-lg shadow-md">
      <el-form
        :model="loginForm"
        :rules="rules"
        ref="loginForm"
        label-width="100px"
        class="space-y-4"
      >
        <el-form-item label="Username" prop="username">
          <el-input
            v-model="loginForm.username"
            placeholder="Username"
          ></el-input>
        </el-form-item>
        <el-form-item label="Password" prop="password">
          <el-input
            type="password"
            v-model="loginForm.password"
            placeholder="Password"
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" class="w-full" @click="handleLogin"
            >Login</el-button
          >
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { ElNotification } from "element-plus";

const loginForm = ref({
  username: "",
  password: "",
});

const rules = {
  username: [
    { required: true, message: "Please input username", trigger: "blur" },
  ],
  password: [
    { required: true, message: "Please input password", trigger: "blur" },
  ],
};

const handleLogin = () => {
  const form = loginForm.value;
  if (form.username && form.password) {
    // Perform login logic here
    ElNotification({
      title: "Success",
      message: "Login successful",
      type: "success",
    });
  } else {
    ElNotification({
      title: "Error",
      message: "Please input username and password",
      type: "error",
    });
  }
};
</script>

<style scoped>
.login-container {
  @apply flex justify-center items-center h-screen bg-gray-100;
}
.login-form {
  @apply w-full max-w-md p-8 space-y-6 bg-white rounded-lg shadow-md;
}
</style>
