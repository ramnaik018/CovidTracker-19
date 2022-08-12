<template>
  <select
    style="
      margin-top: 40px;
      margin-bottom: 40px;
      color: white;
      font-weight: bold;
      border: 5px solid DodgerBlue;
    "
    @change="onChange()"
    v-model="selected"
    class="form-select mt-10 block w-full border p-3 rounded bg-gray-500"
  >
    <option value="0">Select Country</option>
    <option v-for="country in countries" :value="country.ID">
      {{ country.Country }}
    </option>
  </select>
</template>
<script>
export default {
  name: "CountrySelect",
  props: ["countries"],
  data() {
    return {
      selected: 0,
    };
  },
  methods: {
    onChange() {
      const country = this.countries.find((item) => item.ID === this.selected);
      console.log(country);
      this.$emit("get-country", country);
    },
  },
};
</script>
