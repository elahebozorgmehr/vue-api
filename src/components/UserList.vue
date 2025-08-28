<template>
  <div>
    <h2>لیست کاربران</h2>


    <p v-if="loading">در حال بارگذاری...</p>
    <p v-if="error" style="color: red">{{ error }}</p>

    <table v-if="!loading && !error" class="user-table">
      <thead>
        <tr>
          <th>ID</th>
          <th>نام</th>
          <th>نام خانوادگی</th>
          <th>ایمیل</th>
          <th>تلفن</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ user.firstName }}</td>
          <td>{{ user.lastName }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.phone }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const users = ref([]);
const loading = ref(true);
const error = ref("");

onMounted(async () => {
  try {
    const res = await fetch("https://dummyjson.com/users");
    if (!res.ok) throw new Error("خطا در دریافت اطلاعات");

    const data = await res.json();
    users.value = data.users;
  } catch (err) {
    error.value = "خطا در گرفتن اطلاعات کاربران: " + err.message;
    console.error(err);
  } finally {
    loading.value = false;
  }
});
</script>

<style scoped>
.user-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 15px;
}

.user-table th,
.user-table td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: left;
}

.user-table th {
  background-color: #f4f4f4;
  color:#000;
}

h2 {
  margin-bottom: 15px;
}
</style>
