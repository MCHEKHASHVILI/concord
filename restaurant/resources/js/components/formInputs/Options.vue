<template>
  <mdb-container>
    <mdb-select
      v-model="basicOptions"
      placeholder="Select an option"
      label=""
    />
  </mdb-container>
</template>
<script>
  import { mdbSelect, mdbContainer } from "mdbvue";
  export default {
    name: "mdbSelect",
    components: {
      mdbSelect,
      mdbContainer
    },
    data() {
      return {
        basicOptions: [
          { text: "Option nr 1", value: "Option 1" },
          { text: "Option nr 2", value: "Option 2" },
          { text: "Option nr 3", value: "Option 3" }
        ]
      };
    }
  };
</script>