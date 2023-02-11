<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'App',
  mounted: function () {
    this.getItems();
  },
  data: {
    URL: 'http://54.252.3.209:8000/get-reserve-images/',
    reserves: []
  },
  methods: {
    getItems: function () {
      console.log('calling getItems')
      var oReq = new XMLHttpRequest()
      var that = this;
      oReq.addEventListener('load', function () {
        var json = JSON.parse(this.responseText)
        that.reserves = json.data;
      })
      oReq.addEventListener('error', function () {
        console.error('API not working!')
      })
      oReq.open('GET', this.URL);
      oReq.send();
    },
  },
})
</script>

<template>
  <div id="demo">
    <div v-for="res in reserves" >
      <h2>{{ res.title }}</h2>
      <div v-for="(image, i) in res.images" class="container">
        <h4>image #{{ i }}</h4>
        <div class="row">
            <div class="col-sm-4">Master:<img class="img-responsive" :src="image.file"></div>
            <div class="col-sm-4">Full-size copy:<img class="img-responsive" :src="image.original"></div>
            <div class="col-sm-4">Rendition:<img class="img-responsive" :src="image.rendition"></div>
        </div>
      </div>
    </div>
  </div>
</template>
