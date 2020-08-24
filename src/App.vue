<template>
  <v-app>
    <v-main>
      <InterestCalculator />
    </v-main>
  </v-app>
</template>

<script>
import InterestCalculator from "./components/InterestCalculator"

export default {
  name: "App",

  components: {
    InterestCalculator,
  },

  data: () => ({}),
}
</script>
