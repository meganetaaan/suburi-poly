<template>
  <div>
    <a-scene embedded id="scene">
      <a-assets v-if="assets.obj">
        <a-asset-item id="crate-obj" :src="assets.obj"></a-asset-item>
        <a-asset-item id="crate-mtl" :src="assets.mtl"></a-asset-item>
      </a-assets>
      <a-text color="red" position="0 2 -2" :value="message"></a-text>
      <a-obj-model v-if="assets.obj" position="0 0.5 -5" src="#crate-obj" mtl="#crate-mtl"></a-obj-model>
      <a-sky color="#ECECEC"></a-sky>
    </a-scene>
  </div>
</template>

<script>
import 'aframe'

export default {
  name: 'Aframe',
  data () {
    return {
      message: 'Welcome to Your Vue.js App',
      assets: {
        obj: null,
        mtl: null
      }
    }
  },
  mounted () {
    this.retrieveAsset('robot')
  },
  methods: {
    async retrieveAsset (keyword) {
      const res = await this.fetchResources(keyword)
      const json = await res.json()
      const asset = json.assets[0]
      const objUrl = asset.formats[0].root.url
      const mtlUrl = asset.formats[0].resources[0].url
      this.assets.obj = objUrl
      this.assets.mtl = mtlUrl
      console.log(json)
    },
    async fetchResources (keyword) {
      const params = new URLSearchParams()
      params.set('keywords', keyword)
      params.set('curated', true)
      params.set('maxComplexity', 'MEDIUM')
      params.set('format', 'OBJ')
      params.set('orderBy', 'BEST')
      params.set('key', 'AIzaSyD1D4XIpcswKJ5s-XRqsvtS0T9lSA6zUQA')
      return fetch(`https://poly.googleapis.com/v1/assets?${params.toString()}`)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#scene {
  position: absolute;
  width: 100%;
  height: 100%;
}
</style>
