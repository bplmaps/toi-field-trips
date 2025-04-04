<template>
  <div
    v-resize="setRotation"
  >
    <div
      class="absolute inset-0 w-full h-full flex justify-center items-center text-xl font-semibold"
    >
      Loading...
    </div>
    <vl-map
      ref="map"
      class="absolute inset-0 w-full h-full transition-opacity duration-500 ease-in-out bg-white"
      :class="!mapLoaded && 'opacity-0'"
      :load-tiles-while-animating="true"
      :load-tiles-while-interacting="true"
      data-projection="EPSG:4326"
      @created="mapCreated"
    >
      <vl-view :zoom.sync="zoom" :center.sync="center" :rotation.sync="rotation" :max-zoom="maxZoom" />

      <vl-geoloc @update:position="geolocPosition = $event">
        <template #default="geoloc">
          <vl-feature v-if="geoloc.position" id="position-feature">
            <vl-geom-point :coordinates="geoloc.position" />
            <vl-style-box>
              <vl-style-icon src="_media/marker.png" :scale="0.4" :anchor="[0.5, 1]" />
            </vl-style-box>
          </vl-feature>
        </template>
      </vl-geoloc>

      <vl-layer-tile :opacity="1.0">
        <vl-source-xyz :url="maptilerUrl" />
      </vl-layer-tile>


      <vl-feature
        v-for="(location, index) in locations"
        :key="'vl_feature_' + index"
      >
        <vl-geom-point
          :coordinates="[location.longitude, location.latitude]"
        />

        <vl-style>
          <vl-style-circle :radius="10">
            <vl-style-fill
              color="#1f4dfc"
            />
          </vl-style-circle>
        </vl-style>
      </vl-feature>

      <vl-overlay
        v-for="(location, index) in locations"
        :key="'vl_overlay_' + index"
        :position="[location.longitude, location.latitude]"
      >
        <nuxt-link
          :to="'/locations/' + location.slug"
          class="cursor-pointer"
        >
          <span
            class="absolute block transform -translate-x-1/2 -translate-y-1/2"
            :class="(location.slug === lastLocation ? 'map-pulse' : '')"
            :style="{ width: ((zoom / (location.slug === lastLocation ? 8 : 5)) + 'rem'), height: ((zoom / (location.slug === lastLocation ? 8 : 5)) + 'rem') }"
          />
          <span
            class="absolute inline-block transform translate-x-8 -translate-y-1/2 whitespace-nowrap font-bold leading-none rounded-full border-2 border-cobalt p-1 px-2 md:-translate-x-1/2 md:-translate-y-1/2"
            :class="(index % 2 === 0 ? 'md:-mt-12' : 'md:mt-12') + ' ' + (location.slug === lastLocation ? 'bg-white text-cobalt' : 'bg-cobalt text-white')"
          >
            {{ location.title }}
          </span>
        </nuxt-link>
      </vl-overlay>
    </vl-map>

    <div
      v-if="debug"
      class="p-5 bg-black text-white absolute bottom-0 right-0"
    >
      Zoom: {{ zoom }}<br>
      Center: {{ center }}<br>
      Rotation: {{ rotation }}<br>
      My geolocation: {{ geolocPosition }}
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import { mapGetters } from 'vuex'

const breakpoint = 768
const [mobileRotation, desktopRotation] = [-1.5708, 0]

Vue.directive('resize', {
  inserted: (el, binding) => {
    const onResizeCallback = binding.value
    window.addEventListener('resize', () => {
      const width = document.documentElement.clientWidth
      onResizeCallback({ width })
    })
    window.dispatchEvent(new Event('resize'))
  }
})

export default {
  name: 'MapEmbed',
  data () {
    return {
      debug: false,
      mapLoaded: false,
      zoom: 13,
      maxZoom: 19,
      center: [-71.056187, 42.358524],
      extent: [-71.1797,42.2741,-70.9616,42.4305],
      point: [38.726634, 9.003391],
      coordinates: [],
      locations: [],
      rotation: 0,
      maptilerUrl: 'https://api.maptiler.com/maps/toner/{z}/{x}/{y}.png?key=LsmEYuxlnGYrucDaNC45',
      geolocPosition: undefined
    }
  },
  async fetch () {
    this.locations = await this.$content('locations')
      .sortBy('order')
      .fetch()
  },
  computed: {
    ...mapGetters({
      lastLocation: 'locations/getLastLocation'
    })
  },
  methods: {
    mapCreated (vm) {
      this.mapLoaded = true
    },
    setRotation ({ width }) {
      if (width >= breakpoint) {
        this.rotation = desktopRotation
      } else {
        this.rotation = mobileRotation
      }
    }
  }
}
</script>

<style>
.ol-control button {
  background: #1f4dfc !important;
}
</style>

<style lang="postcss" scoped>
.map-pulse::before {
  @apply relative block bg-cobalt rounded-full;

  content: " ";
  width: 200%;
  height: 200%;
  left: -50%;
  top: -50%;
  animation: pulse-ring 1.25s cubic-bezier(0.215, 0.61, 0.355, 1) infinite;
}

.map-pulse::after {
  @apply absolute block bg-white rounded-full w-full h-full inset-0 border-2 border-cobalt;

  content: " ";
  box-shadow: 0 0 8px rgb(0 0 0 / 30%);
  animation: pulse-dot 1.25s cubic-bezier(0.455, 0.03, 0.515, 0.955) -0.4s infinite;
}

@keyframes pulse-ring {
  0% {
    transform: scale(0.5);
  }

  80%,
  100% {
    opacity: 0;
  }
}

@keyframes pulse-dot {
  0% {
    transform: scale(0.8);
  }

  50% {
    transform: scale(1);
  }

  100% {
    transform: scale(0.8);
  }
}
</style>
