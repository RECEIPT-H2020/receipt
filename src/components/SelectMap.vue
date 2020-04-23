<template>
  <v-select :items="Object.values(maps)" v-model="activeMap" @change="selected">
    <template slot="selection" slot-scope="data">
      <!-- HTML that describe how select should render selected items -->
      {{ data.item.name }}
    </template>
    <template slot="item" slot-scope="data">
      <!-- HTML that describe how select should render items when the select is open -->
      {{ data.item.name }}
    </template>
  </v-select>
</template>

<script>
export default {
  name: 'SelectMap',
  data: () => ({
    activeMap: 'sf',
    maps: {
      sf: {
        name: 'San Francisco',
        accessToken:
          'pk.eyJ1IjoiY3R3aG9tZSIsImEiOiJjazhpZ3ljOXUwNTR0M2VxbHhkNWl2NDEzIn0.swIehGzH7mS_TiohUmGuAg',
        style: 'mapbox://styles/mapbox/streets-v11',
        zoom: 12,
        center: [-122.447303, 37.753574],
      },
      amsterdam: {
        name: 'Amsterdam',
        accessToken:
          'pk.eyJ1IjoiY3R3aG9tZSIsImEiOiJjazhpZ3ljOXUwNTR0M2VxbHhkNWl2NDEzIn0.swIehGzH7mS_TiohUmGuAg',
        style: 'mapbox://styles/mapbox/light-v10',
        zoom: 13,
        center: [4.896, 52.37],
      },
    },
  }),
  methods: {
    selected(map) {
      console.log('🎹 Selected map', map)
    },
  },
}
</script>

<style scoped></style>
