<template>
  <div class="flex justify-center">
    <div class="mb-3">
      <input
        class="
          form-control
          block
          w-full
          px-3
          py-1.5
          text-base
          font-normal
          text-gray-700
          bg-white bg-clip-padding
          border border-solid border-gray-300
          rounded
          transition
          ease-in-out
          m-0
          focus:text-gray-700
          focus:bg-white
          focus:border-blue-600
          focus:outline-none
        "
        ref="input_file"
        type="file"
        id="formFile"
        @change="changeToJson($event)"
      />
    </div>
  </div>
</template>
<script>
export default {
  name: "InputFile",
  methods: {
    createKey(stringObj) {
      let lowercase = stringObj.toLowerCase();
      let trimmed = lowercase.trim();
      let value = trimmed.replace(/ /g, "_");

      return value;
    },
    changeToJson(evt) {
      let lines = "";
      let currentLines = "";
      let csv = "";
      let headers = "";
      let result = [];
      let reader = new FileReader();

      reader.readAsBinaryString(this.$refs.input_file.files[0]);
      reader.onload = (evt) => {
        csv = reader.result;
        lines = csv.split("\r" + "\n");
        headers = lines[0].split(",");

        for (let i = 0; i < lines.length; i++) {
          if (!lines[i]) continue;
          let obj = {};
          currentLines = lines[i];
          let re = /"/g;
          currentLines = re[Symbol.replace](currentLines, "");
          currentLines = currentLines.split(",");
          for (let j = 0; j < headers.length; j++) {
            if (j < headers.length) {
              let head = this.createKey(headers[j]);
              let value = currentLines[j].trim();
              obj[head] = value;
            }
          }

          result.push(obj);
        }
        result.shift();

        this.$emit("input", result);
      };
    },
  },
};
</script>