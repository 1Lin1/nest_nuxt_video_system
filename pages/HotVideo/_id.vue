<template>
  <div>
    <h1 class="pa-3">{{course.name}}</h1>

    <!-- 下拉框 -->
    <v-select
    v-model="currentIndex"
    class="pa-3"
    :items="course.episodes.map((v,i) => ({text:v.name,value:i}))"
    >

    </v-select>

    <!-- 视频窗口 -->
    <div style="border:1px solid #000000"> 
      <video 
    
    :src="episode.file"
    width="100%"
    height="100%"
    
    controls
   
    >
    </video>
    </div>

    
  </div>
</template>

<script>
    export default {
        async asyncData({
            params,
            $axios
        }) {
            const {
                id
            } = params;
            const course = await $axios.$get(`courses/${ id }`, {
                params: {
                    query: {
                        populate: 'episodes'
                    }
                }
            })

            return {
                course: course
            }
        },

        data() {
            return {
                currentIndex: 0
            }
        },
        computed: {
            episode() {
                return this.course.episodes[this.currentIndex]
            }
        },



    }
</script>