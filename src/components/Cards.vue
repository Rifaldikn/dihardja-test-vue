<script setup>
defineProps({
  item: Object,
});
</script>

<template>
  <v-card
    min-height="150"
    variant="outlined"
    rounded="xl"
    width="auto"
    class="pa-5"
  >
    <v-row>
      <v-col cols="6">
        <div>{{ item.name }}</div>
        <div>{{ item.place }}</div>
        <div>{{ item.dob }}</div>
      </v-col>
      <v-col cols="6" class="d-flex justify-end">
        <v-avatar rounded="lg" size="100">
          <img cover aspect-ratio="1" :src="item.image" alt="alt" />
        </v-avatar>
      </v-col>
      <v-col cols="12">
        <v-divider thickness="2"></v-divider>
      </v-col>
      <v-col class="d-flex justify-space-between align-center">
        <div>Status</div>
        <div class="d-flex align-center space-between">
          <v-icon :color="item.isOnline ? 'green' : 'grey'" size="small"
            >mdi-circle</v-icon
          >{{ item.isOnline ? "Online" : "Offline" }}
        </div>
      </v-col>
    </v-row>
  </v-card>
</template>
