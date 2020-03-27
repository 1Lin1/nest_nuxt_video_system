<template>
  <div>
    <h1 class="pa-3">热门视频</h1>


    <v-row>
      <v-col md="4" v-for="item in courses" :key="item._id">
        <v-card :to="`HotVideo/${item._id}`" class="mx-auto single-card" max-width="350">
          <v-img :src="item.videoCover" height="200px"></v-img>

          <v-card-title>
            {{item.videoName}}
          </v-card-title>

          <v-card-subtitle>
            {{item.videoContent}}
          </v-card-subtitle>

          <v-card-actions>
            <v-btn text>Share</v-btn>

            <v-btn color="purple" text>
              Explore
            </v-btn>

            <v-spacer></v-spacer>

            <!-- <v-btn icon @click="show = !show">
          <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
        </v-btn> -->
          </v-card-actions>

          <!-- <v-expand-transition>
        <div v-show="show">
          <v-divider></v-divider>

          <v-card-text>
            I'm a thing. But, like most politicians, he promised more than he
            could deliver. You won't have time for sleeping, soldier, not with
            all the bed making you'll be doing. Then we'll go with that data
            file! Hey, you add a one and two zeros to that or we walk! You're
            going to do his laundry? I've got to find a way to escape.
          </v-card-text>
        </div>
      </v-expand-transition> -->
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
    export default {
        async asyncData({
            $axios
        }) {
            let coursesData = await $axios.$get('courses')
            console.log(coursesData.data)
            return {
                courses: coursesData.data
            }


        }
    }
</script>

<style scoped lang="scss">
    .single-card {
        transition: all 0.5s;
        &:hover {
            transform: translate(0, -20px);
        }
    }
</style>>

</style>