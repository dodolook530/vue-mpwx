<template>
  <div>
    <div v-show="!files.length" class="upload_area webuploader-container">
      <a class="btn btn_upload js_vote_upload_btn webuploader-pick">
        上传文件
      </a>
      <ul class="upload_file_box" style="display:none"></ul>
      <div style="position: absolute; top: 0px; left: 0px; width: 106px; height: 32px; overflow: hidden; bottom: auto; right: auto;">
        <label style="opacity: 0; width: 100%; height: 100%; display: block; cursor: pointer; background: rgb(255, 255, 255);">
          <input v-el:input type="file" style="display: none;"
            :accept="accept" :name="name"
            :multiple="multiple" @change="change">
        </label>
      </div>
    </div>
    <div v-if="files.length" class="img_container">
      <span v-for="item in files" class="img_panel">
        <img class="preview" :src="item">
      </span>
      <a href="javascript:;" class="link_dele" @click="clear()">删除</a>
    </div>
  </div>
</template>

<script>
  /* global FileReader */
  const forEach = Array.prototype.forEach

  export default {
    props: {
      accept: {
        type: String,
        default: 'image/*'
      },
      name: {
        type: String
      },
      multiple: {
        type: Boolean
      },
      files: {
        type: Array,
        default: () => []
      }
    },

    methods: {
      change (e) {
        // todo: html5 filereader
        // todo: img_container
        this.files = []
        const files = e.target.files
        console.log('files', files)
        forEach.call(files, (item, i) => {
          const reader = new FileReader()
          reader.readAsDataURL(item)
          reader.onload = (e) => {
            this.files.$set(i, e.target.result)
          }
        })
      },

      clear () {
        this.files = []
        this.$els.input.value = ''
        // this.$els.input.files = null
      }
    }
  }
</script>

<style scoped>
  .upload_area {
    position: relative;
    display: inline-block;
    vertical-align: middle;
  }
  .btn.btn_upload {
    background-color: #fff;
    background-image: -moz-linear-gradient(top,#fff 0,#fff 100%);
    background-image: -webkit-gradient(linear,0 0,0 100%,from(#fff),to(#fff));
    background-image: -webkit-linear-gradient(top,#fff 0,#fff 100%);
    background-image: -o-linear-gradient(top,#fff 0,#fff 100%);
    background-image: linear-gradient(to bottom,#fff 0,#fff 100%);
    border-color: #e7e7eb;
    color: #222;
    height: 30px;
    line-height: 30px;
    width: auto;
    padding-left: 22px;
    padding-right: 22px;
  }
  .upload_file_box {
    position: absolute;
    top: 100%;
    left: 0;
    white-space: nowrap;
    background-color: #fff;
    box-shadow: none;
    -moz-box-shadow: none;
    -webkit-box-shadow: none;
    border: 1px solid #e7e7eb;
    border-radius: 0;
    -moz-border-radius: 0;
    -webkit-border-radius: 0;
  }
  .upload_file_box {
    margin-top: -1px;
    padding: 5px 0;
    max-height: 300px;
  }
  /*.vote_edit .upload_area .upload_file_box*/
  .upload_area .upload_file_box {
    z-index: 4;
    right: 0;
    left: auto;
  }
  .frm_control_group .link_delete {
    margin-left: .5em;
  }

  .img_container {
    margin-left: 5em;
    margin-top: .5em;
  }
  .frm_controls .img_container {
    margin-left: 0;
  }
  .img_container .img_panel {
    display: table-cell;
  }
  .img_container img {
    width: 75px;
    height: 75px;
    display: block;
  }
  .img_container .link_dele {
    display: table-cell;
    padding-left: .5em;
  }
</style>
