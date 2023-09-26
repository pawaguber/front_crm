<template>

  <v-btn
      color="primary"
      size="large"
  >
    Создать роль
    <CreateRoleModal/>
  </v-btn>
  <VDataTable
      :headers="headers"
      :items="roles"
      :items-per-page="25"
  >
    <template v-slot:item.action="{ item }">
      <v-btn
          color="primary"
          icon="mdi mdi-pencil-outline"
          @click="openModal(item.key)"
      >
      </v-btn>
    </template>
  </VDataTable>

  <RoleModal v-if="selectedId" :id="selectedId.value"/>
</template>

<script setup>
import {onMounted} from 'vue';
import {VDataTable} from 'vuetify/labs/VDataTable'
import axios from 'axios';
import CreateRoleModal from "../../../components/dashboard/roles/CreateRoleModal.vue";
import {login} from "../../../store/login.js";
import RoleModal from "../../../components/dashboard/roles/RoleModal.vue";

const roles = ref([]);
const selectedId = ref(null);

const openModal = (id) => {
  selectedId.value = id;
  console.log(selectedId.value)
};

const headers = [
  {
    title: 'ID',
    sortable: false,
    key: 'id',
  },
  {
    title: 'Назва',
    key: 'name',
  },
  {
    title: 'Порядок',
    key: 'order_by',
  },
  {
    title: 'Група',
    key: 'type',
  },
  {
    title: 'Стан',
    key: 'state',
  },
  {
    title: 'Дія',
    key: 'action',
    sortable: false,
  },
]

onMounted(async () => {
  const token = localStorage.getItem('accessToken');

  try {
    const response = await axios.get('http://127.0.0.1:8000/api/role', {
      headers: {
        Accept: "application/json",
        Authorization: `Bearer ${token}`
      }
    });
    roles.value = response.data.data;
    login(roles)
  } catch (error) {
    console.error("Ошибка при выполнении запроса:", error);
  }
});


</script>

<style scoped>
h1 {
  font-size: 24px;
}

.modal {
  /* тут ви можете додати стилі для модального вікна */
}
</style>
