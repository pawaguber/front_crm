<template>
  <v-dialog
      v-model="dialog"
      activator="parent"
      width="auto"
  >
    <v-card>
      <v-row>
        <v-col
            cols="6"
            md="6"
        >
          <v-btn
            color="info"
          >
            Редагувати
          </v-btn>
        </v-col>

        <v-col
            cols="6"
            md="6"
        >
            <DeleteRoleModal :id="props.id"  />
        </v-col>
      </v-row>
      <v-card-actions>
        <v-btn color="primary" block @click="toggleDialog">Закрити</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script setup>
import {ref} from 'vue';
import DeleteRoleModal from "./DeleteRoleModal.vue";

const props = defineProps({
  id: Number
});

console.log(props)

const dialog = ref(false);

const toggleDialog = () => {
  dialog.value = !dialog.value;
}
</script>
