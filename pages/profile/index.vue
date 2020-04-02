<template>
  <div>
    <v-card class="mx-auto" max-width="80%">  
      <v-img
        src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
        height="400px"
      ></v-img>

      <v-card-title> 欢迎您 {{ userInfo.username }} </v-card-title>

      <v-card-subtitle>
        1,000 miles of wonder
      </v-card-subtitle>

      <v-card-actions>
        <v-btn text>分享</v-btn>

        <v-btn color="purple" text>
          探索
        </v-btn>

        <v-spacer></v-spacer>

        <v-btn icon @click="show = !show">
          <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
        </v-btn>
      </v-card-actions>

      <v-expand-transition>
        <div v-show="show">
          <v-divider></v-divider>

          <v-card-text>
            我是有底线的
          </v-card-text>
        </div>
      </v-expand-transition>

      <v-divider></v-divider>

        <!-- //用户个人信息 -->
    <profile-form></profile-form> 
    </v-card>

  
  </div>
</template>

<script>
import { mapState, mapMutations } from 'vuex'
import profileForm from '../../components/profile/profileForm'
export default {
  components: {
    profileForm
  },
  data: () => ({
    show: false
  }),

  computed: {
    userInfo() {
      return this.$store.state.auth.user
    }
  }
}
</script>
