<template>
  <div class="home">
    <div id="map"></div>

    <div class="panel">
      <v-container style="pointer-events: all;">
        <v-btn @click="isCreatingStory = !isCreatingStory" :color="isCreatingStory ? 'primary' : ''"
          >Create story</v-btn
        >
        <v-btn @click="setStyle">Set style</v-btn>
        <v-btn @click="fly">Fly</v-btn>

        <!-- story scroll-->
        <scroll-stories />

        <v-sheet v-if="!isCreatingStory" class="mt-3">
          <v-container>
            <div class="title">Stories (From DB)</div>

            <div v-for="story in stories" :key="story.id">
              <v-row>
                <v-col cols="3">
                  <v-btn><v-icon left>mdi-play</v-icon> Play</v-btn>
                </v-col>
                <v-col>
                  <div>{{ story.title }}</div>
                  <p>{{ story.description }}</p>
                </v-col>
              </v-row>
            </div>
          </v-container>
        </v-sheet>

        <!--Create a story-->
        <v-sheet v-if="isCreatingStory" class="mt-3">
          <v-container>
            <div class="title">New Story</div>
            <v-text-field label="Story name" required outlined></v-text-field>
            Camera:
            <v-row>
              <v-col>
                <pre class="caption">{{ camera }}</pre>
              </v-col>
              <v-col><v-btn>Import GeoJson</v-btn> <v-btn class="mt-2">Import CSV</v-btn></v-col>
            </v-row>

            <create-story />
          </v-container>
        </v-sheet>
      </v-container>
    </div>
  </div>
</template>

<script>
// import ToggleTileBoundariesControl from "./MapboxControls";
import CreateStory from '@/components/createStory'
import ScrollStories from '@/components/ScrollStories'
import gql from 'graphql-tag'

const mapboxgl = require('mapbox-gl')

const getCamera = (map) => ({
  center: map.getCenter(),
  bearing: map.getBearing(),
  zoom: map.getZoom(),
})

export default {
  name: 'Mapbox',
  components: { ScrollStories, CreateStory },
  mounted() {
    // build the style url
    const url = 'mapbox://styles/mapbox/streets-v11'
    // instantiate Mapbox GL
    mapboxgl.accessToken =
      'pk.eyJ1IjoiY3R3aG9tZSIsImEiOiJjazhpZ3ljOXUwNTR0M2VxbHhkNWl2NDEzIn0.swIehGzH7mS_TiohUmGuAg'

    const map = new mapboxgl.Map({
      container: 'map',
      style: url,
      hash: true,
    })
    this.map = map

    /**
     * See all events here https://docs.mapbox.com/mapbox-gl-js/api/#map.event
     */
    map.on('load', () => {
      console.log('🎹 Map is ready')
      this.camera = getCamera(map)
      // Add navigation control
      // There are more Controls and overlays:
      // https://docs.mapbox.com/mapbox-gl-js/examples/#controls-and-overlays
      let nav = new mapboxgl.NavigationControl()
      map.addControl(nav, 'bottom-left')
    })
    map.on('styledata', () => {})
    map.on('moveend', (event) => {
      console.log('🎹', event)
      this.camera = getCamera(map)
    })
  },
  data() {
    return {
      map: null,
      camera: null,
      isCreatingStory: false,
      mapActions: {},
      activeMap: null,
      popupCoordinates: [4.899, 52.372],
    }
  },
  methods: {
    setStyle() {
      this.map.setStyle('mapbox://styles/ctwhome/ck9b6yhm00cgm1iqsmunkcxi4')
    },
    fly() {
      this.map.flyTo({
        center: [18.96 + (Math.random() - 0.5) * 20, 44.85 + (Math.random() - 0.5) * 20],
        essential: true, // this animation is considered essential with respect to prefers-reduced-motion
      })
    },
  },
  apollo: {
    stories: gql`
      query stories {
        stories {
          id
          story
          title
          description
        }
      }
    `,
  },
}
</script>

<style>
#map {
  position: absolute;
  top: 0px;
  left: 0;
  width: 100vw;
  height: calc(100vh - 48px);
}

.panel {
  position: absolute;
  top: 0;
  right: 0;
  width: 40vw;
  height: calc(100vh - 48px);
  background: #00000070;
  backdrop-filter: blur(5px);
  /*pointer-events: none;*/
  /*box-shadow: -20px 0px 20px rgba(0, 0, 0, 0.1);*/
  overflow-y: auto;
}

@media only screen and (max-width: 600px) {
  #map {
    height: calc(50vh - 48px);
  }
  .panel {
    top: 50%;
    right: 0;
    width: 100%;
    height: calc(50vh - 64px);
    backdrop-filter: none;
    background: #00000095;
  }
  #features {
    display: none;
  }
}

#features {
  font-family: sans-serif;
  overflow-y: scroll;
  background-color: #fafafa;
  /*height: calc(100vh - 248px);*/
  height: 300px;
  margin-top: 40px;
  width: 100%;
}
section {
  padding: 25px 50px;
  line-height: 25px;
  border-bottom: 1px solid #ddd;
  opacity: 0.25;
  font-size: 13px;
}

section:last-child {
  border-bottom: none;
  margin-bottom: 200px;
}
</style>
