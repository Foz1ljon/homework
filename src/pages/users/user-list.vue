<template>
  <div class="mb-20">
    <div class="flex items-center justify-between mb-10">
      <h1>User list</h1>
      <el-button type="primary" @click="router.push('/create-user')"
        >Create User</el-button
      >
    </div>

    <el-table v-loading="loading" :data="users" style="width: 100%">
      <el-table-column type="index" :index="indexMethod" width="50" />
      <el-table-column prop="firstName" label="First Name" />
      <el-table-column prop="lastName" label="Last Name" />
      <el-table-column prop="age" label="Age" />
      <el-table-column prop="phone" label="Phone" />
      <el-table-column fixed="right" label="Operations">
        <template #default="scope">
          <el-button
            type="primary"
            size="small"
            @click="router.push(`/edit-user/${scope.row.id}`)"
          >
            <el-icon>
              <Edit />
            </el-icon>
          </el-button>
          <el-popconfirm
            @confirm="delUser(scope.row.id)"
            title="Are you sure to delete this?"
          >
            <template #reference>
              <el-button type="danger" size="small">
                <el-icon>
                  <Delete />
                </el-icon>
              </el-button>
            </template>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>

    <div class="flex justify-end mt-10">
      <el-pagination
        v-model="currentPage"
        @current-change="handleCurrentChange"
        background
        layout="total,sizes, prev, pager, next"
        :total="totalUsers"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import api from "../../api";
import { useRouter } from "vue-router";
import { ElNotification } from "element-plus";

const router = useRouter();
const users = ref([]);
const loading = ref(false);
const totalUsers = ref(0);
const pageSize = ref(10);
const currentPage = ref(1);

const handleCurrentChange = (page) => {
  currentPage.value = page;
  fetchUsers();
};

const indexMethod = (index) => {
  return (currentPage.value - 1) * pageSize.value + index + 1;
};

const fetchUsers = () => {
  loading.value = true;
  api
    .get("/users", {
      params: {
        limit: pageSize.value,
        skip: (currentPage.value - 1) * pageSize.value,
      },
    })
    .then((res) => {
      users.value = res.data.users;
      totalUsers.value = res.data.total;
    })
    .catch((err) => {
      console.log("err", err);
    })
    .finally(() => {
      loading.value = false;
    });
};

fetchUsers();

/// Delete User by id
const delUser = async (id) => {
  api
    .delete(`/users/${id}`)
    .then(() => {
      console.log("success!");
    })
    .catch((err) => {
      console.log(err.message);
    });
  fetchUsers();
  ElNotification({
    title: "Successfully delete user",
    type: "success",
  });
};
</script>

<style scoped></style>
