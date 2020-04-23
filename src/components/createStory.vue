<template>
  <v-form v-model="valid">
    <v-sheet class="mt-3">
      <v-container class="pb-0">
        <div class="title">New Story</div>
        <v-text-field
          class="mt-2"
          dense
          label="Story name"
          required
          outlined
          :rules="titleRules"
        ></v-text-field>
      </v-container>
    </v-sheet>
    <v-container justify-content="end">
      <v-btn disabled color="primary"><v-icon left>mdi-plus</v-icon> Add Page</v-btn>
    </v-container>
    <v-sheet>
      <v-container>
        <div class="title mb-2">Page 1</div>

        <v-text-field
          v-model="titlePage"
          :rules="nameRules"
          :counter="20"
          label="Page Name"
          required
          outlined
          dense
        ></v-text-field>
        <rich-editor />
        <div class="caption mt-3"><v-icon size="13">mdi-video-vintage</v-icon> {{ camera }}</div>

        <v-row no-gutters class="mt-3">
          <v-btn>Import GeoJson</v-btn>
          <v-btn class="ml-3">Import CSV</v-btn>
        </v-row>
      </v-container>
    </v-sheet>

    <v-container class="mt-3">
      <v-row no-gutters>
        <v-btn class="mr-2" @click="$emit('close')">cancel</v-btn>
        <v-btn disabled class="mr-2" color="primary">Save</v-btn>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
import RichEditor from './RichEditor'
import gql from 'graphql-tag'

export default {
  name: 'createStory',
  components: { RichEditor },
  props: {
    camera: {
      type: Object,
      default: () => {},
    },
  },
  data: () => ({
    valid: false,
    title: '',
    titlePage: '',
    description: 'A description',
    titleRules: [(v) => !!v || 'Title is required'],
    nameRules: [
      (v) => !!v || 'Name is required',
      (v) => v.length <= 20 || 'Name must be less than 10 characters',
    ],
    email: '',
    emailRules: [
      (v) => !!v || 'E-mail is required',
      (v) => /.+@.+/.test(v) || 'E-mail must be valid',
    ],
  }),
  apollo: {
    stories: gql`
      query stories {
        stories {
          id
          title
          description
        }
      }
    `,
  },
  // apollo: {
  //   addStory: () => {
  //     this.$apollo.mutate({
  //       mutation: gql`
  //         mutation createStory {
  //           insert_stories(objects: { title: "Other Story", description: "new description" }) {
  //             affected_rows
  //           }
  //         }
  //       `,
  //       // variables: {
  //       //   title: this.title,
  //       //   description: this.description,
  //       // },
  //       // update: (cache, { data: { insert_todos } }) => {
  //       //   // Read the data from our cache for this query.
  //       //   // eslint-disable-next-line
  //       //   console.log(insert_todos)
  //       // },
  //     })
  //   },
  // },
}
</script>

<style scoped></style>
