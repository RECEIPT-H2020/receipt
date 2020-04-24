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

    <div v-for="(page, index) in pages" :key="index">
      <stoy-page :page="{ camera: page.camera }" :camera="camera" />
    </div>

    <v-container class="mt-3">
      <div class="d-flex">
        <div class="flex">
          <v-btn color="amber"><v-icon left>mdi-plus</v-icon> Add Page</v-btn>
        </div>
        <div>
          <v-btn class="mr-2" @click="$emit('close')">cancel</v-btn>
          <v-btn class="mr-2" color="primary">Save</v-btn>
        </div>
      </div>
    </v-container>
  </v-form>
</template>

<script>
import gql from 'graphql-tag'
import StoyPage from './StoyPage'

export default {
  name: 'createStory',
  components: { StoyPage },
  props: {
    camera: {
      type: Object,
      default: () => {},
    },
  },
  data: () => ({
    valid: false,
    title: '',
    description: 'A description',

    pages: [
      {
        styleMapUrl: '',
        titlePage: '',
      },
    ],

    titleRules: [(v) => !!v || 'Title is required'],
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
