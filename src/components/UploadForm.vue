<template>
  <div class="form-group">
    <form @submit.prevent="onSubmit">
      <input
        type="file"
        class="form-control"
        accept="image/png, image/jpeg"
        @change="onChange"
      />
      <button :disabled="!isValid" type="submit" class="btn btn-primary">
        Submit
      </button>
    </form>
    <pre>{{ fileData }}</pre>
    <pre>{{ urlData }}</pre>
    <pre>{{ source }}</pre>
    <img :src="source" />
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "upload-form",
  data() {
    return {
      file: null,
      fileData: null,
      source: "",
      urlData: null
    };
  },
  computed: {
    isValid() {
      return this.fileData !== null;
    }
  },
  methods: {
    async onChange($event) {
      try {
        const {
          target: {
            files: [file]
          }
        } = $event;
        this.file = file;
        const { name, type } = file;
        this.fileData = { name, type };
        const {
          data: { key, url }
        } = await axios.get(
          `http://localhost:3000/files?name=${name}&type=${type}`
        );
        this.urlData = { key, url };
      } catch (err) {
        console.error(err);
      }
    },
    async onSubmit() {
      try {
        const { type } = this.fileData;
        const { key, url } = this.urlData;
        const [id] = key.split(".");
        await axios.put(url, this.file, {
          headers: { "Content-Type": type }
        });
        await axios.post(`http://localhost:3000/files/${id}`);
        this.source = `https://s3.eu-west-3.amazonaws.com/files.dev.gamelearn.io/${key}`;
      } catch (err) {
        console.error(err);
      }
    }
  }
};
</script>

<style scoped></style>
