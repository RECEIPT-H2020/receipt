<template>
  <v-form v-model="valid">
    <div class="title">Page {{ firstname }}</div>
    <v-container>
      <v-row>
        <v-col cols="12" md="6">
          <v-text-field
            v-model="firstname"
            :rules="nameRules"
            :counter="20"
            label="Page Name"
            required
            outlined
          ></v-text-field>
        </v-col>

        <!--        <v-col cols="12" md="4">-->
        <!--          <v-text-field-->
        <!--            v-model="lastname"-->
        <!--            :rules="nameRules"-->
        <!--            :counter="10"-->
        <!--            label="Last name"-->
        <!--            required-->
        <!--            outlined-->
        <!--          ></v-text-field>-->
        <!--        </v-col>-->

        <!--        <v-col cols="12" md="4">-->
        <!--          <v-text-field v-model="email" :rules="emailRules" label="E-mail" required></v-text-field>-->
        <!--        </v-col>-->
      </v-row>

      <rich-editor />
      <v-row>
        <v-btn color="Save" class="mr-2">Save</v-btn>
        <v-btn color="primary">Add New Page</v-btn>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
import RichEditor from './RichEditor'
export default {
  name: 'createStory',
  components: { RichEditor },
  data: () => ({
    valid: false,
    firstname: '',
    lastname: '',
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
}
</script>

<style scoped></style>
