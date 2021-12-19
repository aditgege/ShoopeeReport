<template>
  <div id="app">
    <div class="container">
      <div class="flex flex-col mx-auto w-11/12">
        <InputFile @input="setResult"></InputFile>
        <div class="mb-4">
          <button
            @click="detail = !detail"
            type="button"
            class="
              px-6
              py-2.5
              bg-green-500
              text-white
              font-medium
              text-xs
              leading-tight
              uppercase
              rounded
              shadow-md
              hover:bg-green-600 hover:shadow-lg
              focus:bg-green-600 focus:shadow-lg focus:outline-none focus:ring-0
              active:bg-green-700 active:shadow-lg
              transition
              duration-150
              ease-in-out
            "
          >
            Show Detail
          </button>
        </div>
        <DataList v-if="detail" :items="result" />
        <Status :result="result"></Status>
      </div>
    </div>
  </div>
</template>

<script>
import DataList from "../src/components/DataList.vue";
import InputFile from "../src/components/InputFile.vue";
import Status from "../src/components/Status.vue";

export default {
  name: "App",
  components: {
    DataList,
    InputFile,
    Status
  },
  data() {
    return {
      result: [],
      detail: false,
    };
  },
  methods: {
    setResult(result) {
      this.result = result;
    },
  },
};
</script>

<style>
#app {
  margin-top: 60px;
}
</style>
