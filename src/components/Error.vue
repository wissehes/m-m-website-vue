<template>
  <v-alert prominent type="error">
    <v-row align="center">
      <v-col class="grow">Kon niet verbinden met de server.</v-col>
      <v-col class="shrink">
        <v-btn @click="load">Opnieuw proberen</v-btn>
      </v-col>
    </v-row>
  </v-alert>
</template>

<script>
export default {
  props: [
    'load'
  ]
}
</script>
