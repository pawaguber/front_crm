<template>
  <div class="text-center">
    <v-btn color="primary" @click="dialog = true">
      Видалити
    </v-btn>

    <v-dialog v-model="dialog" width="auto">
      <v-card>
        <v-card-text>
          Ви дійсно хочете видалити роль?
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" block @click="deleteRole">Так</v-btn>
          <v-btn color="secondary" block @click="dialog = false">Ні</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const props = defineProps({
  id: Number
});

const dialog = ref(false);
console.log('heello')
console.log(props)

const deleteRole = async () => {
  try {
    const token = localStorage.getItem('accessToken');
    await axios.delete(`http://127.0.0.1:8000/api/role/${props.id}`, {
      headers: {
        Accept: "application/json",
        Authorization: `Bearer ${token}`
      }
      }).then(response => {
      console.log(response.data)
    });
    dialog.value = false;
  } catch (error) {
    console.error(error);
  }
};
</script>
