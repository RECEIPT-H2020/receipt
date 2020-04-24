<template>
  <v-sheet class="mt-2">
    <v-container>
      <div class="title mb-2 flex">Page 1</div>

      <v-text-field
        v-model="page.titlePage"
        :rules="titlePageRules"
        :counter="20"
        label="Page Name"
        required
        outlined
        dense
      ></v-text-field>
      <rich-editor />

      <div class="d-flex align-center">
        <div class="font-weight-bold mt-3 flex">Map Options:</div>
        <div class="caption">
          Open
          <a href="https://studio.mapbox.com/" target="_blank"
            >Mapbox studio
            <v-icon size="13">mdi-open-in-new</v-icon>
          </a>
        </div>
      </div>

      <div class="caption mt-3">
        <v-icon size="13">mdi-video-vintage</v-icon>
        {{ camera }}
      </div>

      <v-text-field
        v-model="page.styleMapUrl"
        label="Style URL"
        placeholder="mapbox://styles"
        outlined
        dense
        class="mt-3"
      ></v-text-field>
      <v-text-field
        v-model="page.accessToken"
        label="Access token"
        placeholder="pk..."
        outlined
        dense
        class="mt-3"
      ></v-text-field>
      <v-row no-gutters class="mt-3">
        <v-btn>Import GeoJson</v-btn>
        <!--          <v-btn class="ml-3">Import CSV</v-btn>-->
      </v-row>
    </v-container>
  </v-sheet>
</template>
<script>
import RichEditor from './RichEditor'

export default {
  name: 'stoy-page',
  components: { RichEditor },
  props: {
    camera: String,
    page: {
      type: Object,
      default: () => ({
        camera: '',
        styleMapUrl: '',
        titlePage: '',
      }),
    },
  },
  data: () => ({
    titlePageRules: [
      (v) => !!v || 'Name is required',
      (v) => v.length <= 20 || 'Name must be less than 10 characters',
    ],
  }),
}
</script>
