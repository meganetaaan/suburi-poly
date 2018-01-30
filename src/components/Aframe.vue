<template>
  <div>
    <a-scene embedded id="scene">
      <a-assets v-if="assets.gltf">
        <a-asset-item id="gltf" :src="assets.gltf"></a-asset-item>
      </a-assets>
      <a-sky color="#ECECEC"></a-sky>
      <a-light type="ambient"></a-light>
      <a-text color="red" position="0 2 -2" :value="message"></a-text>
      <a-gltf-model v-if="assets.gltf" position="1 1 -2" src="#gltf"></a-gltf-model>
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
        gltf: null
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
      const gltfModel = asset.formats.find((f) => {
        return f.formatType === 'GLTF2'
      })
      this.assets.gltf = gltfModel.root.url
      console.log(json)
    },
    async fetchResources (keyword) {
      const params = new URLSearchParams()
      params.set('keywords', keyword)
      params.set('curated', true)
      params.set('maxComplexity', 'MEDIUM')
      params.set('format', 'GLTF2')
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
