<template>
  <v-row
    justify="center"
    align="center">
    <v-col md="10">
      <v-carousel
        cycle
        hide-delimiter-background
        show-arrows-on-hover>
        <v-carousel-item
          v-for="(item, i) in items"
          :key="i"
          :src="item.src"
        ></v-carousel-item>
      </v-carousel>
    </v-col>
  </v-row>
</template>

<script>
export default {
  components: {

  },
  data(){
    return {
      items:[
        {src:'https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=1875807258,18485981&fm=26&gp=0.jpg'},
        {src:'https://dss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3435142851,1686079016&fm=26&gp=0.jpg'},
      ]
    }
  }
}
</script>
