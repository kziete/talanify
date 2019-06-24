<template>
  <div class="talana-image-upload">
    <!-- slot for parent component to activate the file changer -->
    <div class="talana-image-upload-wrapper" @click="launchFilePicker()">
      <slot name="activator"></slot>
    </div>
    <slot name="actions"></slot>
    <!-- image input: style is set to hidden and assigned a ref so that it can be triggered -->
    <input type="file"
           ref="file"
           :name="uploadFieldName"
           @change="onFileChange($event.target.name, $event.target.files)"
           style="display:none">
    <!-- error dialog displays any potential errors -->
    <v-dialog v-model="errorDialog" max-width="300">
      <v-card>
        <v-card-text class="subheading">{{errorText}}</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn @click="errorDialog = false" flat>Got it!</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
  export default {
    name: 'UploadPhoto',
    props: {
      // Use "value" here to enable compatibility with v-model
      value: Object,
    },
    data: () => ({
      errorDialog: null,
      errorText: '',
      uploadFieldName: 'file',
      maxSize: 1024
    }),
    methods: {
      launchFilePicker () {
        this.$refs.file.click();
      },
      onFileChange (fieldName, file) {
        const {maxSize} = this
        let imageFile = file[0]
        //check if user actually selected a file
        if (file.length > 0) {
          let size = imageFile.size / maxSize / maxSize
          if (!imageFile.type.match('image.*')) {
            // check whether the upload is an image
            this.errorDialog = true
            this.errorText = 'Please choose an image file'
          } else if (size > 2) {
            // check whether the size is greater than the size limit
            this.errorDialog = true
            this.errorText = 'La imagen es demasiado grande :( por favor agrega una menor a 2MB'
          } else {
            // Append file into FormData & turn file into image URL
            let formData = new FormData()
            let imageURL = URL.createObjectURL(imageFile)
            formData.append('file', imageFile)
            // Emit FormData & image URL to the parent component
            this.$emit('input', {formData, imageURL})
          }
        }
      }
    },
    watch: {
      value: function (nV, pV) {
        if (nV === null) {
          this.$refs.file.value = '';
        }
      }
    }
  }
</script>

<style lang="scss">
  .talana-image-upload {
    position: relative;

    .talana-edit-image {
      position: absolute;
      bottom: 0;
      right: 0;
      margin: 0;
      transform: scale(.9);
    }

    .talana-delete-image {
      position: absolute;
      right: -47px;
      bottom: -7px;
      transform: scale(.9);
    }

    &-wrapper > div {
      border-radius: 50%;
      padding: 2px;
      background: var(--remuneraciones-color);

      & > * {
        border: 2px solid #fff;

        & > [class*="mdi"]:before {
          font-size: 50px;
        }
      }
    }
  }
</style>
