<template>
  <div class="mt-32 relative mb-10 h-full max-w-7xl mx-auto w-full px-8">
    <div class="flex items-center justify-between mt-6">
      <h1 class="font-medium text-base sm:text-xl text-gray-900">
        Uploading
      </h1>

      <a
        @click="isUploading = !isUploading"
        class="font-normal focus:outline-none cursor-pointer flex items-center"
      >
        <span
          @click="cancelUploads(file)"
          class="hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase  text-base sm:text-base text-blue-400"
        >
          Cancel Upload
        </span>

        <ChevronUpArrowIcon
          @click="isUploading = !isUploading"
          class="ml-4 text-gray-900 "
          style="40px"
          v-if="!isUploading"
        />
        <ChevronDownArrowIcon class="ml-4 text-gray-900 " style="40px" v-else />
      </a>
    </div>

    <!-- border  -->
    <div class="border mt-3 mb-6 border-gray-200 max-w-7xl mx-auto"></div>

    <!-- Vue Dropzone file upload -->
    <div class="" v-show="isUploading">
      <vue-dropzone
        id="dropzone"
        class="bg-gray-100  hover:bg-gray-200 hover:border-gray-200 cursor-pointer hover:text-gray-600 px-4 rounded-md shadow-sm hover:shadow-sm transition-all duration-300"
        ref="myVueDropzone"
        :useCustomSlot="true"
        :options="dropzoneOptions"
        @vdropzone-error="errorFiles"
        @vdropzone-removed-file="vremoved"
        @vdropzone-success="successFiles"
        @vdropzone-files-added="filesAddedHandler"
        @vdropzone-total-upload-progress="progressHandler"
      >
        <div class="dropzone-custom-content">
          <h3 class="dropzone-custom-title">
            Drag and drop to upload File content!
          </h3>
          <div class="subtitle">
            ...or click to select a file from your computer
          </div>
        </div>
      </vue-dropzone>
    </div>

    <!-- Next up file uploading section -->
    <div class="mt-10">
      <div class="flex items-center justify-between">
        <h1 class="font-medium text-base sm:text-xl text-gray-900">
          Next Up
        </h1>

        <a
          @click="isNextUp = !isNextUp"
          class="font-normal focus:outline-none hover:focus:outline cursor-pointer flex items-center"
        >
          <span
            @click="cancelAll(file)"
            class="hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase text-base sm:text-base text-blue-400"
          >
            Cancel all
          </span>

          <ChevronUpArrowIcon
            @click="isNextUp = !isNextUp"
            class="ml-4 text-gray-900 "
            style="40px"
            v-if="!isNextUp"
          />
          <ChevronDownArrowIcon
            class="ml-4 text-gray-900 "
            style="40px"
            v-else
          />
        </a>
      </div>

      <!-- border  -->
      <div class="border mt-3 mb-6 border-gray-200 max-w-7xl mx-auto"></div>

      <div v-show="isNextUp">
        <div
          v-for="(file, index) in pendingFileList"
          :key="index"
          class="bg-gray-100 hover:bg-gray-200 hover:border-gray-400 rounded-md shadow-sm px-6 py-4 h-32 mb-6"
        >
          <div class="relative" @click="removePendingFile(file.file)">
            <XIcon
              class="-top-2 absolute cursor-pointer disabled h-4 hover:text-blue-500 -right-3 w-8 z-10"
            />
          </div>

          <!-- File details -->
          <div class="flex space-x-6 items:center justify-between">
            <div class="flex items-center space-x-6">
              <img :src="file.file" alt="" class="w-20 h-20" />

              <!-- file (name & size) -->
              <div class="">
                <div class=" font-normal truncate text-gray-900 text-base">
                  {{ file.file.name }}
                </div>

                <div class="mt-2 font-normal text-gray-400 text-sm">
                  {{ file.file.size }} MB
                </div>
              </div>
            </div>

            <!-- Loading State -->
            <div class="mt-3 pr-12">
              <div class="spinner-border text-secondary" role="status"></div>
              <div class="font-normal text-gray-400 text-sm -ml-3 mt-1">
                Waiting
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Completed uploads Section -->
    <div class="mt-10">
      <div class="flex items-center justify-between">
        <h1 class="font-medium text-base sm:text-xl text-gray-900">
          Completed
        </h1>

        <a
          @click="isCompleted = !isCompleted"
          class="font-normal focus:outline-none hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-blue-400"
        >
          <span
            @click="dismiss(file)"
            v-if="successFilesList.length"
            class="text-blue-400 cursor-pointer text-base hover:underline hover:text-blue-500"
          >
            DISMISS
          </span>
          <ChevronUpArrowIcon
            @click="isCompleted = !isCompleted"
            class="ml-4 text-gray-900 "
            style="40px"
            v-if="!isCompleted"
          />
          <ChevronDownArrowIcon
            class="ml-4 text-gray-900 "
            style="40px"
            v-else
          />
        </a>
      </div>

      <!-- border  -->
      <div class="border mt-3 mb-6 border-gray-200 max-w-7xl mx-auto"></div>

      <div v-if="isCompleted && successFilesList.length">
        <div
          v-for="(file, index) in successFilesList"
          :key="index"
          class="bg-gray-100 hover:bg-gray-200 hover:border-gray-400 rounded-md shadow-sm px-6 py-4 h-32 mb-6"
        >
          <div class="relative" @click="removePendingFile(file.file)">
            <XIcon
              class="-top-2.5 -right-3 absolute cursor-pointer disabled h-4 hover:text-blue-500 w-8 z-10"
            />
          </div>
          <!-- file details -->
          <div class="flex space-x-6 items:center justify-between">
            <div class="flex items-center space-x-6">
              <img :src="file.file.dataURL" alt="" class="w-12 h-12" />

              <!-- file (name & size) -->
              <div class="">
                <div class=" font-normal truncate text-gray-900 text-base">
                  {{ file.file.name }}
                </div>

                <div class="mt-2 font-normal text-gray-400 text-sm">
                  {{ file.file.size }} MB
                </div>
              </div>
            </div>

            <!-- complete State -->
            <div class="mt-3 pr-10">
              <CheckMarkIcon class="w-6 h-6" />
              <div class="font-normal text-gray-400 text-sm -ml-1 mt-1">
                Done
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="font-normal text-gray-400 text-sm mt-2" v-else>
        No file uploads completed yet.
      </div>
    </div>

    <!-- Incomplete Uploading Section -->
    <div class="mt-10">
      <div class="flex items-center justify-between">
        <h1 class="font-medium text-base sm:text-xl text-gray-900">
          Incomplete Uploads
        </h1>

        <button
          @click="isIncomplete = !isIncomplete"
          class="font-normal focus:outline-none hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-blue-400"
        >
          <ChevronUpArrowIcon
            @click="isIncomplete = !isIncomplete"
            class=" ml-4 text-gray-900 "
            style="40px"
            v-if="!isIncomplete"
          />
          <ChevronDownArrowIcon
            class="ml-3 text-gray-900 "
            style="40px"
            v-else
          />
        </button>
      </div>

      <!-- border  -->
      <div class="border mt-3 mb-6 border-gray-200 max-w-7xl mx-auto"></div>

      <div v-if="isIncomplete && rejectedFilesList.length">
        <div
          class="flex items-center justify-between"
          v-if="rejectedFilesList.length"
        >
          <label
            class="font-normal text-sm text-blue-500 hover:underline cursor-pointer"
          >
            RETRY ALL
          </label>

          <label
            @click="dismissAll(file)"
            class="font-normal text-sm text-blue-500 hover:underline cursor-pointer"
          >
            DISMISS All
          </label>
        </div>

        <div
          v-for="(file, index) in rejectedFilesList"
          :key="index"
          class="bg-gray-100 mt-3 hover:bg-gray-200 hover:border-gray-400 rounded-md shadow-sm px-6 py-4 h-32 mb-6"
        >
          <div class="relative " @click="removePendingFile(file.file)">
            <XIcon
              class="-top-2.5 absolute cursor-pointer disabled h-4 hover:text-blue-500 -right-3 w-8 z-10"
            />
          </div>
          <!-- file details -->
          <div class="flex items:center justify-between">
            <div class="flex items-center space-x-6">
              <img :src="file.file.dataURL" alt="" class="w-20 h-20" />

              <!-- file name & size -->
              <div>
                <div class=" font-normal truncate text-gray-900 text-base">
                  {{ file.file.name }}
                </div>

                <div class="mt-2 font-normal text-gray-400 text-sm">
                  {{ file.file.size }} MB
                </div>
              </div>
            </div>
            <!-- failed State -->
            <div class="mt-3.5 pr-10">
              <RetryIcon class="w-6 h-6" />
              <div class="font-normal text-gray-400 text-sm -ml-1 mt-2">
                Failed
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="font-normal text-gray-400 text-sm sm:text-base mt-2" v-else>
        No incomplete upoads yet.
      </div>
    </div>
  </div>
</template>

<script>
import vue2Dropzone from 'vue2-dropzone'
import 'vue2-dropzone/dist/vue2Dropzone.min.css'
import ChevronDownArrowIcon from './icons/ChevronDownArrowIcon.vue'
import ChevronUpArrowIcon from './icons/ChevronUpArrowIcon.vue'
import XIcon from './icons/XIcon.vue'
import CheckMarkIcon from './icons/CheckMarkIcon.vue'
import RetryIcon from './icons/RetryIcon.vue'
export default {
  name: 'App',
  components: {
    vueDropzone: vue2Dropzone,
    ChevronDownArrowIcon,
    ChevronUpArrowIcon,
    XIcon,
    CheckMarkIcon,
    RetryIcon,
  },
  data: function() {
    return {
      dropzoneOptions: {
        url: 'https://httpbin.org/post',
        thumbnailWidth: 150,
        maxFilesize: 1.5,
        headers: { 'My-Awesome-Header': 'header value' },
        acceptedFiles: 'image/*',
        addRemoveLinks: true,
      },
      isUploading: true,
      isNextUp: true,
      isCompleted: true,
      isIncomplete: true,
      fileId: 1,
      success: false,
      error: false,
      removedFile: false,
      pendingFileList: [],
      successFilesList: [],
      rejectedFilesList: [],
    }
  },

  watch: {
    removedFile() {
      let that = this
      setTimeout(function() {
        that.removedFile = false
      }, 2000)
    },
  },

  methods: {
    // Success Completed uploads file
    async successFiles(file) {
      this.successFilesList = [
        ...this.successFilesList,
        { id: file?.lastModified, file },
      ]
      this.pendingFileList = this.pendingFileList?.filter(
        (f) => f.id != file?.lastModified
      )
      this.$refs?.myVueDropzone?.removeFile(file)
    },

    cancelUploads() {
      this.pendingFileList = []
    },

    cancelAll() {
      this.pendingFileList = []
    },

    dismiss() {
      this.successFilesList = []
    },

    dismissAll() {
      this.rejectedFilesList = []
    },

    // Incomplete files
    errorFiles(file) {
      this.rejectedFilesList = [
        ...this.rejectedFilesList,
        { id: file?.lastModified, file },
      ]
    },

    // Add push files uploading
    filesAddedHandler(files) {
      for (let i = 0; i < files?.length; i++) {
        this.pendingFileList.push({
          id: files[i]?.lastModified,
          file: files[i],
        })
        this.isCompleted =
          true || this.isIncomplete == true || this.isNextUp == true
      }
    },

    // Remove file
    vremoved() {
      this.removedFile = true
    },

    removePendingFile(file) {
      // remove pending waiting next up file list
      const index = this.pendingFileList.findIndex(
        (v) => v.file.name === file.name
      )
      if (index >= 0) {
        this.pendingFileList.splice(index, 1)
        alert('Your file removed successfully.')
        return true
      }

      // remove rejected error file lists
      const index1 = this.rejectedFilesList.findIndex(
        (v) => v.file.name === file.name
      )
      if (index1 >= 0) {
        this.rejectedFilesList.splice(index, 1)
        alert('Your file removed successfully.')
        return true
      }
      // remove success completed file list
      const index2 = this.successFilesList.findIndex(
        (v) => v.file.name === file.name
      )
      if (index2 >= 0) {
        this.successFilesList.splice(index, 1)
        alert('Your file removed successfully.')
        return true
      }
    },

    // Remove Pending files
    removePendings() {
      for (let i = 0; i < this.pendingFileList?.length; i++) {
        // remove from drag and drop zone
        this.$refs?.myVueDropzone?.removeFile(this.pendingFileList[i])
        // remove from pending
        console.log(this.pendingFileList[i])
        this.pendingFileList = this.pendingFileList?.filter(
          (f) => f?.id == this.pendingFileList[i]?.lastModified
        )
      }
    },

    // Progress bar status
    progressHandler(totaluploadprogress, totalBytes, totalBytesSent) {
      console.log('totaluploadprogress ', totaluploadprogress)
      console.log('totalBytes ', totalBytes)
      console.log('totalBytesSent ', totalBytesSent)
    },
  },
}
</script>

<style lang="css">
.truncate {
  width: 80px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.vue-dropzone {
  border: 2px solid rgb(212, 212, 212);
  font-family: Arial, sans-serif;
  letter-spacing: 0.2px;
  color: #777;
  background: rgba(246, 247, 252, 1);
  transition: 0.2s linear;
}
.vue-dropzone::hover() {
  border: 2px solid rgb(175, 175, 175);
  font-family: Arial, sans-serif;
  background: rgb(153, 152, 152);
  letter-spacing: 0.2px;
  color: #777;
  transition: 0.2s linear;
}
.dropzone {
  min-height: 150px;
  background: #f6f7fc;
  /* padding: 20px 20px; */
}
.vue-dropzone {
  border: white;
  font-family: Arial, sans-serif;
  letter-spacing: 0.2px;
  color: #777;
  transition: 0.1s linear;
}

.rounded-md {
  border-radius: 0.5rem;
}

.vue-dropzone:hover {
  background-color: #e2e7ea94;
  border: 2px solid #dadada;
}
.vue-dropzone > .dz-preview .dz-remove {
  z-index: 30;
  margin-left: 12px;
  padding: 10px;
  position: relative;
  /* border: 2px #d3d3d3 solid; */
  text-decoration: none;
  text-transform: uppercase;
  font-size: 0.8rem;
  font-weight: 800;
  letter-spacing: 1.1px;
  opacity: 0;
}

.dropzone .dz-preview .dz-remove {
  font-size: 14px;
  text-align: center;
  display: block;
  cursor: pointer;
  border: none;
  margin-top: 22px;
}
.vue-dropzone > .dz-preview .dz-remove {
  position: relative;
  z-index: 30;
  color: black;
  margin-left: 1px;
  padding: 10px;
  border: 2px black solid;
  text-decoration: none;
  text-transform: uppercase;
  font-size: 0.8rem;
  font-weight: 800;
  letter-spacing: 1.1px;
  opacity: 0;
  margin-top: 26px;
}

.vue-dropzone > .dz-preview .dz-image {
  border-radius: 0;
  width: 42%;
  height: 100%;
}

.dropzone .dz-preview.dz-image-preview {
  background: transparent;
}
.dropzone .dz-preview .dz-details {
  z-index: 20;
  position: absolute;
  left: 78px;
  display: block;
  color: black;
  opacity: 0;
  font-size: 13px;
  min-width: 100%;
  max-width: 100%;
  padding: 2em 1em;
  text-align: center;
  color: rgba(0, 0, 0, 0.9);
  line-height: 150%;
}
.vue-dropzone > .dz-preview .dz-details {
  bottom: 0;
  top: 0;
  color: black;
  background-color: transparent;
  transition: opacity 0.2s linear;
  /* text-align: left; */
}
.dropzone .dz-preview .dz-progress {
  opacity: 1;
  z-index: 1000;
  pointer-events: none;
  position: absolute;
  height: 40px;
  left: 34%;
  top: 24%;
  margin-top: -8px;
  width: 40px;
  margin-left: -40px;
  background: transparent;
  -webkit-transform: scale(1);
  border-radius: 50px;
  border: 3px solid lightgray;
  overflow: hidden;
}
.dropzone .dz-preview {
  position: relative;
  display: inline-block;
  vertical-align: top;
  margin: 10px 34px -3px;
  min-height: 100px;
}

/* loading icon */
.text-secondary {
  color: #c1c5c9 !important;
}

.spinner-border {
  display: inline-block;
  width: 1.5rem;
  height: 1.5rem;
  vertical-align: text-bottom;
  border: 3px solid currentColor;
  border-right-color: transparent;
  border-radius: 50%;
  -webkit-animation: spinner-border 0.75s linear infinite;
  animation: spinner-border 0.75s linear infinite;
}

/* error message  */

.dropzone .dz-preview.dz-error .dz-error-message {
  display: block;
  margin-top: 2px;
}
.vue-dropzone > .dz-preview .dz-error-message {
  text-align: center;
}
.dropzone .dz-preview .dz-error-message {
  pointer-events: none;
  z-index: 1000;
  position: absolute;
  display: block;
  display: none;
  opacity: 0;
  transition: opacity 0.3s ease;
  border-radius: 8px;
  font-size: 10px;
  top: 50px;
  left: -1px;
  width: 140px;
  background: #be2626;
  background: linear-gradient(to bottom, #be2626, #a92222);
  padding: 0.5em 1.2em;
  color: white;
}
.vue-dropzone > .dz-preview .dz-error-mark,
.vue-dropzone > .dz-preview .dz-success-mark {
  margin-left: auto;
  margin-top: auto;
  width: 100%;
  top: 35%;
  left: 0;
  display: none;
}
</style>
