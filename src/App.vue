<template>
  <div id="app">
    <!-- upload -->
    <div>
      <vue-dropzone
        id="dropzone"
        ref="myVueDropzone"
        :useCustomSlot="true"
        :options="dropzoneOptions"
        @vdropzone-error="errorFiles"
        @vdropzone-success="successFiles"
        @vdropzone-files-added="filesAddedHandler"
        @vdropzone-total-upload-progress="progressHandler"
      >
        <div class="dropzone-custom-content">
          <h3 class="dropzone-custom-title">
            Drag and drop to upload content!
          </h3>
          <div class="subtitle">
            ...or click to select a file from your computer
          </div>
        </div>
      </vue-dropzone>
    </div>
    <!-- nextup -->
    <div></div>
    <!-- completed -->
    <div></div>
    <!-- error -->
    <div></div>
  </div>
</template>

<script>
import vue2Dropzone from "vue2-dropzone";
import "vue2-dropzone/dist/vue2Dropzone.min.css";

export default {
  name: "App",
  components: {
    vueDropzone: vue2Dropzone,
  },
  data: function() {
    return {
      dropzoneOptions: {
        url: "https://httpbin.org/post",
        thumbnailWidth: 150,
        // maxFilesize: 0.5,
        headers: { "My-Awesome-Header": "header value" },
      },
      fileId: 1,
      pendingFileList: [],
      successFilesList: [],
      rejectedFilesList: [],
    };
  },
  methods: {
    async successFiles(file) {
      this.successFilesList = [
        ...this.successFilesList,
        { id: file?.lastModified, file },
      ];
      this.pendingFileList = this.pendingFileList?.filter(
        (f) => f.id != file?.lastModified
      );
      this.$refs?.myVueDropzone?.removeFile(file);
    },
    errorFiles(file) {
      this.rejectedFilesList = [
        ...this.rejectedFilesList,
        { id: file?.lastModified, file },
      ];
    },
    filesAddedHandler(files) {
      for (let i = 0; i < files?.length; i++) {
        this.pendingFileList.push({
          id: files[i]?.lastModified,
          file: files[i],
        });
      }
    },
    removePendings() {
      for (let i = 0; i < this.pendingFileList?.length; i++) {
        // remove from drag and drop zone
        this.$refs?.myVueDropzone?.removeFile(this.pendingFileList[i]);
        // remove from pending
        console.log(this.pendingFileList[i]);
        this.pendingFileList = this.pendingFileList?.filter(
          (f) => f?.id == this.pendingFileList[i]?.lastModified
        );
      }
    },
    progressHandler(totaluploadprogress, totalBytes, totalBytesSent) {
      console.log("totaluploadprogress ", totaluploadprogress);
      console.log("totalBytes ", totalBytes);
      console.log("totalBytesSent ", totalBytesSent);
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
