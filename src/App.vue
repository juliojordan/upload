<template>
  <div id="app" class="container py-4">
    <div class="row">
      <div class="col-12">
        <upload-form></upload-form>
      </div>
    </div>
  </div>
</template>

<script>
import "bootstrap/dist/css/bootstrap.min.css";
import UploadForm from "./components/UploadForm.vue";

export default {
  name: "app",
  components: {
    UploadForm
  }
};
</script>

<style></style>
