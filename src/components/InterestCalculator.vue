<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="10" offset="1">
        <v-card :loading="loading" outlined style="border: 1px solid grey">
          <v-card-title>Interest Calculator</v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    :rules="rules.number"
                    prefix="$"
                    v-model="values.present"
                    label="Present Value"
                    outlined
                  ></v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col cols="7">
                  <v-text-field
                    :rules="rules.number"
                    suffix="%"
                    v-model="values.insertRate"
                    label="Insert Rate"
                    outlined
                  ></v-text-field>
                </v-col>
                <v-col cols="5">
                  <v-select :items="items.insertRateTypes" v-model="values.insertRateType" outlined></v-select>
                </v-col>
              </v-row>
              <v-row>
                <v-col cols="7">
                  <v-text-field :rules="rules.number" v-model="values.term" label="Term" outlined></v-text-field>
                </v-col>
                <v-col cols="5">
                  <v-select :items="items.termTypes" v-model="values.termType" outlined></v-select>
                </v-col>
              </v-row>
              <v-row>
                <v-col>
                  <v-btn :disabled="disabled" color="primary" large @click="calculate()">Calculate</v-btn>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row v-show="showResult">
      <v-col cols="10" offset="1">
        <v-card outlined style="border: 1px solid grey">
          <v-card-title>Calculation Result</v-card-title>
          <v-card-text>If you invest ${{values.present}} now, {{values.term}} {{values.termType}} later, you get ${{result}}</v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "InterestCalculator",

  data: () => ({
    rules: {
      number: [(val) => !isNaN(val) || "You must write a number!"],
    },
    items: {
      insertRateTypes: ["Yearly", "Monthly", "Daily"],
      termTypes: ["Year", "Month", "Day"],
    },
    values: {
      insertRateType: "Yearly",
      insertRate: 0,
      termType: "Month",
      term: 0,
      present: 0,
    },
    loading: false,
    showResult: false,
    result: 0,
  }),
  methods: {
    calculate() {
      this.loading = true
      this.result = (
        this.values.present *
        Math.pow(1 + this.values.insertRate * 0.01, this.termTypeTransform)
      ).toFixed(2)
      this.showResult = true
      this.loading = false
    },
  },
  computed: {
    termTypeTransform() {
      if (this.values.insertRateType === "Yearly") {
        if (this.values.termType === "Year") return this.values.term
        else if (this.values.termType === "Month") return this.values.term / 12
        else if (this.values.termType === "Day") return this.values.term / 360
      } else if (this.values.insertRateType === "Monthly") {
        if (this.values.termType === "Year") return this.values.term * 12
        else if (this.values.termType === "Month") return this.values.term
        else if (this.values.termType === "Day") return this.values.term / 360
      } else if (this.values.insertRateType === "Daily") {
        if (this.values.termType === "Year") return this.values.term * 360
        else if (this.values.termType === "Month") return this.values.term * 30
        else if (this.values.termType === "Day") return this.values.term
      }
      return false
    },
    disabled() {
      if (
        this.values.insertRate != 0 &&
        this.values.term != 0 &&
        this.values.present != 0
      )
        return false
      else return true
    },
  },
  watch: {
    values: {
      handler() {
        this.showResult = false
      },
      deep: true,
    },
  },
}
</script>
