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
        {src:'https://img3.doubanio.com/view/photo/l/public/p2565594632.webp'},
        {src:'https://img9.doubanio.com/view/photo/l/public/p2565594636.webp'},
        {src:'https://img9.doubanio.com/view/photo/l/public/p2565594635.webp'},
        {src:'https://img3.doubanio.com/view/photo/l/public/p2565594631.webp'},
      ]
    }
  }
}
</script>
