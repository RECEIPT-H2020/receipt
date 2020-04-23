<template>
  <vue-ckeditor
    v-model="htmlContent"
    :config="{ ...editorConfig, placeholder }"
    :editors="editors"
    @keyup="updateHtmlContent"
    @change="updateHtmlContent"
    type="classic"
    placeholder="Message"
    ref="editor"
  />
</template>

<script>
import VueCkeditor from 'vue-ckeditor5'
import ClassicEditor from '@ckeditor/ckeditor5-build-classic'
// import BuildInline from '@ckeditor/ckeditor5-build-inline'
// import { compressImage, toFirebaseStore, deleteImage } from '../utils/uploadImages'

// import Emoji from '@wwalc/ckeditor5-emoji/src/emoji'
// import EssentialsPlugin from '@ckeditor/ckeditor5-essentials/src/essentials'
// import BoldPlugin from '@ckeditor/ckeditor5-basic-styles/src/bold'
// import ItalicPlugin from '@ckeditor/ckeditor5-basic-styles/src/italic';
// import LinkPlugin from '@ckeditor/ckeditor5-link/src/link';
// import ParagraphPlugin from '@ckeditor/ckeditor5-paragraph/src/paragraph';
//
export default {
  name: 'RichEditor',
  components: {
    'vue-ckeditor': VueCkeditor.component,
  },
  props: {
    placeholder: { type: String, default: '' },
    initContent: { type: String, default: '' },
    completeButtons: { type: Boolean, default: false },
  },
  data() {
    return {
      textColor: 'blue',
      htmlContent: '',
      editors: {
        classic: ClassicEditor,
      },
      editorConfig: {
        toolbarLocation: 'bottom',
        startupFocus: true,
        ignoreEmptyParagraph: true,
        // toolbar: ['bold', 'italic', '|', 'link', 'undo', 'redo', 'InsertImage', 'paragraph'],

        // plugins: [
        // Emoji,
        // EssentialsPlugin,
        // BoldPlugin,
        //   ItalicPlugin,
        //   LinkPlugin,
        //   ParagraphPlugin
        // ],
        //
        toolbar: {
          items: this.completeButtons
            ? [
                'heading',
                'bold',
                'italic',
                '|',
                'link',
                'bulletedList',
                'numberedList',
                'imageUpload',
                'blockQuote',
                'insertTable',
                'mediaEmbed',
                'undo',
                'redo',
              ]
            : [
                // 'heading',
                'bold',
                'italic',
                // '|',
                'link',
                'bulletedList',
                // 'numberedList',
                'imageUpload',
                // 'blockQuote',
                // 'insertTable',
                // 'mediaEmbed',
                // 'undo',
                // 'redo',
              ],
        },
      },
    }
  },

  mounted() {
    this.htmlContent = this.initContent

    // TODO: when entering in edit, put hte focus on the text area
    // this.$nextTick(() => this.$refs.editor.focus())
    //   console.log('🎹', this.$refs.editor.$el)
    //   this.$refs.editor.$el.focus()
    // console.log('🎹', this.$refs?.editor?.focus())
  },
  methods: {
    /**
     * Send contentHtml to the parent
     */
    updateHtmlContent() {
      this.cleanEmptyParragraphs()
      this.$emit('updateHtmlContent', this.htmlContent)
    },

    clear() {
      this.htmlContent = ''
      this.updateHtmlContent()
    },
    cleanEmptyParragraphs() {
      // console.log(this.htmlContent);
      // <p>&nbsp;</p>
    },

    /**
     * CKEditor Uploader
     * @param loader
     * @constructor
     */
    /*UploadAdapter: function (loader) {
      this.loader = loader
      this.upload = async () => {
        const body = new FormData()
        body.append('file', this.loader.file)

        return compressImage(await this.loader.file).then((result) => {
          return toFirebaseStore(result)
            .then((response) => {
              return { default: response }
            })
            .catch((error) => console.log(error))
        })
      }
      this.abort = () => {
        console.log('Abort upload.')
      }
    },*/

    /**
     * Delete image from storage
     */
    /* deleteImageFromStorage() {
      deleteImage
        .then(() => {
          this.uploading = false
          this.uploadEnd = false
          this.downloadURL = ''
        })
        .catch((error) => {
          console.error(`file delete error occured: ${error}`)
        })
    },*/
  },
}
</script>

<style lang="scss">
/*.dark {*/
.ck-editor {
  color: var(--themed-text);
}
.ck {
  .ck-content {
    background: var(--themed-bg-lighten) !important;
  }
  .ck-button {
    color: var(--themed-text);
  }
}
/*}*/
</style>
