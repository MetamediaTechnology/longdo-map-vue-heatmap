<template>
  <div id="app">
    <div id="map"></div>
  </div>
</template>

<script>
import subwayData from "@/assets/data/subway.json";
import * as h337 from "heatmap.js";

export default {
  name: "App",
  data() {
    return {
      map: null,
      subwayData: subwayData,
      heatmapLayer: {},
      heatmapcfg: {
        backgroundColor: "#000000",
        radius: 10,
        maxOpacity: 0.8,
        // minOpacity: 1,
        scaleRadius: false,
        useLocalExtrema: true,
      },
    };
  },
  components: {},
  mounted() {
    this.initMap();
  },
  methods: {
    initMap() {
      this.map = new window.longdo.Map({
        placeholder: document.getElementById("map"),
      });
      this.map.location({ lat: 23.714532, lon: 88.543282 });
      this.map.zoom(2);
      this.createHeatMap();
    },
    createHeatMap() {
      const LongdoMapHeatMap = this.LongdoHeatMap.heatmap(h337);
      this.heatmapLayer = new LongdoMapHeatMap(this.heatmapcfg);
      let HeatmapData = [];

      this.subwayData.features.forEach((x) => {
        HeatmapData.push({
          lat: x.properties.SUBWAYLAT,
          lon: x.properties.SUBWAYLONG,
          value: 2,
        });
      });
      // Add locations for creating heatmap layer
      this.heatmapLayer.setData({ data: HeatmapData });
      this.map.Layers.add(this.heatmapLayer);
      // if you need to remove layer please use map.Layers.remove(heatmapLayer)
    },
  },
  computed: {
    LongdoHeatMap() {
      return require("@/assets/resources/heatmap/vue-longdomap-heatmap.js")
        .default;
    }
  },
};
</script>

<style>
html,
body,
#app {
  margin: 0px;
  height: 100%;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#map {
  height: 100%;
}
</style>
