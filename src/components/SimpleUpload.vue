<template>
  <form @submit.prevent="sendFile" enctype="multipart/form-data">
    <div
      v-if="message"
      :class="`message ${error ? 'is-danger' : 'is-success'}`"
    >
      <div class="message-body">{{ message }}</div>
    </div>

    <div class="field">
      <div class="file is-boxed is-primary">
        <label class="file-label">
          <input
            type="file"
            ref="file"
            @change="selectFile"
            class="file-input"
          />
          <span class="file-cta">
            <span class="file-icon">
              <i class="fas fa-upload"> </i>
            </span>
            <span class="file-label"> Choose a file ... </span>
          </span>
          <span v-if="file" class="file-name">{{ file.name }}</span>
        </label>
      </div>
    </div>

    <div class="field">
      <button class="button is-info">Send</button>
    </div>
  </form>
</template>




<script>
import axios from "axios";

export default {
  name: "SimpleUpload",

  data() {
    return {
      file: "",
      message: "",
      error: "",
    };
  },

  methods: {
    selectFile() {
      //   console.log(this.$refs.file.files[0]);
      this.file = this.$refs.file.files[0];
      this.error = "";
      this.message = "";
    },

    async sendFile() {
      const formData = new FormData();
      formData.append("file", this.file);

      try {
        await axios({
          method: "POST",
          url: "http://localhost:3000/upload",
          data: formData,
        });

        this.message = "File has been uploaded";
        this.error = false;
        this.file = "";
      } catch (error) {
        console.log(error);
        this.error = true;
        this.message = "Something went wrong!";
      }
    },
  },
};
</script>
