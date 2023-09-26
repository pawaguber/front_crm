<template>

  <v-dialog
      :width="$vuetify.display.smAndDown ? 'auto' : 600 "
      v-model="dialog"
      activator="parent"
      width="auto"
  >
    <v-card>
      <v-form v-model="valid" ref="createForm" @submit.prevent="onSubmit">
        <v-container>
          <v-row>
            <v-col
                cols="12"
            >
              <v-text-field
                  v-model="data.name"
                  :counter="10"
                  label="Назва"
                  required
                  hide-details
              ></v-text-field>

              <v-select
                  label="Група"
                  :items="groups.data"
                  item-value="id"
                  item-title="name"
                  variant="solo-inverted"
                  v-model="data.type"
              ></v-select>
              <v-switch
                  label="Стан (активно\не активно)"
                  v-model="data.state"
              ></v-switch>
            </v-col>
          </v-row>
          <v-alert
              v-if="info.show"
              :type="info.status"
          >
            {{ info.message }}
          </v-alert>

          <v-row>
            <v-col
                col="12"
                md="6"
            >
              <v-btn
                  color="success"
                  type="Створити"
              >
                Створити
              </v-btn>
            </v-col>

            <v-col
                col="12"
                md="6"
            >
              <v-btn
                  color="primary"
                  size="small"
                  block @click="dialog = false">

                Закрити
              </v-btn>
            </v-col>
          </v-row>
        </v-container>
      </v-form>

    </v-card>
  </v-dialog>
</template>

<script setup>
import {onMounted} from "vue";
import axios from "axios";


const alertStatus = ref(null);
const alertMessage = ref('');
const dialog = ref(false)
const createForm = ref(null)
const valid = ref(true)
const data = ref({
  'name': '',
  'type': '',
  'state': 1,
  'order_by': 15
})
const groups = ref([])
const info = ref({
  show: true,
  status: '',
  message: '',
})

const toggleDialog = () => {
  dialog.value = !dialog.value;
}

const onSubmit = () => {
  if (valid) {
    const token = localStorage.getItem('accessToken');
    axios.post('http://127.0.0.1:8000/api/role', data.value, {
      headers: {
        'Authorization': `Bearer ${token}`
      }
    })
        .then(({ data: { data: { message } } }) => {
          info.value.message = message;
          info.value.status = 'success';
          info.value.show = true;
          createForm.value.reset()
        })
        .catch(({ response }) => {
          console.log(response)
          const { status, data: { message } } = response;
          if (status === 422) {
            info.value.message = message;
            info.value.status = 'error';
            info.value.show = true;
          }
        })
  }
}

onMounted(async () => {
  const token = await localStorage.getItem('accessToken');

  try {
    const response = await axios.get('http://127.0.0.1:8000/api/role/group', {
      headers: {
        Accept: "application/json",
        Authorization: `Bearer ${token}`
      }
    })
    groups.value = response.data;
    console.log(groups)
  } catch (error) {
    console.error(error)
  }
})
</script>


<style scoped>

</style>