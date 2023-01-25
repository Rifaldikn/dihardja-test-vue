<script setup>
import UserCard from "./Cards.vue";

import { ref, onMounted } from "vue";

import { useVuelidate } from "@vuelidate/core";
import { required, minLength } from "@vuelidate/validators";

const baseItem = {
  name: "",
  dob: "",
  place: "",
  isOnline: true,
  image: "",
};

const rules = {
  name: {
    required,
    minLength: minLength(8),
  },
  dob: { required },
  place: { required },
};

const lists = ref([]);
const showList = ref(true);
const forms = ref({});

const v$ = ref(useVuelidate(rules, forms));

async function addData() {
  const isFormValid = await v$.value.$validate();

  console.log(isFormValid);

  if (isFormValid) {
    lists.value.push({
      ...forms.value,
      image: getRandomImage(),
    });

    populateLists();
  }
}

function populateLists() {
  forms.value = { ...baseItem };
  v$.value.$reset();

  console.log("reset");
}

function getRandomImage() {
  let randomInt = Math.ceil(Math.random() * 10);
  return `https://picsum.photos/id/${randomInt}/200/300`;
}

function clearData() {
  lists.value = [];
}

function getSingleError(error) {
  return error.map((e) => e.$message).join(", ");
}

onMounted(() => {
  populateLists();
});
</script>

<template>
  <v-container class="pa-10">
    <v-row dense>
      <v-col cols="12">
        <div>Name</div>
        <v-text-field
          single-line
          class="elevation-0 rounded-lg"
          density="compact"
          variant="solo"
          flat
          clearable
          bg-color="#F6F6F6"
          v-model.trim="forms.name"
          :error-messages="getSingleError(v$.name.$errors)"
          @blur="v$.name.$touch"
        ></v-text-field>
      </v-col>

      <v-col cols="12">
        <div>Date of Birth</div>
        <v-text-field
          single-line
          class="elevation-0 rounded-lg"
          density="compact"
          variant="solo"
          clearable
          flat
          bg-color="#F6F6F6"
          :error-messages="getSingleError(v$.dob.$errors)"
          v-model="forms.dob"
          @blur="v$.dob.$touch"
        ></v-text-field>
      </v-col>

      <v-col cols="12">
        <div>Place</div>
        <v-text-field
          single-line
          class="elevation-0 rounded-lg"
          density="compact"
          variant="solo"
          clearable
          flat
          bg-color="#F6F6F6"
          v-model.trim="forms.place"
          :error-messages="getSingleError(v$.place.$errors)"
          @blur="v$.place.$touch"
        ></v-text-field>
      </v-col>

      <v-col cols="12">
        <v-radio-group inline v-model="forms.isOnline">
          <v-radio
            class="mr-5"
            color="primary"
            label="Online"
            :value="true"
          ></v-radio>
          <v-radio color="primary" label="Offline" :value="false"></v-radio>
        </v-radio-group>
      </v-col>

      <v-col class="d-flex justify-center">
        <v-btn
          flat
          rounded="lg"
          class="text-capitalize mx-5"
          color="primary"
          @click="addData"
          >Add Data
        </v-btn>
        <v-btn
          flat
          rounded="lg"
          variant="outlined"
          class="mx-5 text-capitalize"
          color="success"
          @click="showList = !showList"
          >{{ showList ? "Hide Data" : "Show Data" }}
        </v-btn>
        <v-btn
          flat
          rounded="lg"
          class="text-capitalize mx-5"
          color="success"
          @click="clearData"
        >
          Clear Data
        </v-btn>
      </v-col>
    </v-row>

    <v-row v-show="showList">
      <template v-if="lists.length > 0">
        <v-col v-for="card in lists" cols="12" sm="12" md="6" lg="4">
          <UserCard :item="card" />
        </v-col>
      </template>
      <v-col v-else cols="12" class="text-center">
        <v-card
          height="300"
          variant="outlined"
          rounded="lg"
          class="d-flex align-center justify-center"
        >
          <h3 class="text-grey">No Data</h3>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
