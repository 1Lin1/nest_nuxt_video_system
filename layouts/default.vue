<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
      clipped
    >
      <v-list dense>
        <v-list-item
          v-for="item in items"
          :key="item.text"
          :to="item.link"
          link
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>
              {{ item.text }}
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-subheader class="mt-4 grey--text text--darken-1">SUBSCRIPTIONS</v-subheader>
        <v-list>
          <v-list-item
            v-for="item in items2"
            :key="item.text"
            link
          >
            <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
            <v-list-item-title v-text="item.text" />
          </v-list-item>
        </v-list>
        <v-list-item
          class="mt-4"
          link
        >
          <v-list-item-action>
            <v-icon color="grey darken-1">mdi-plus-circle-outline</v-icon>
          </v-list-item-action>
          <v-list-item-title class="grey--text text--darken-1">Browse Channels</v-list-item-title>
        </v-list-item>
        <v-list-item link>
          <v-list-item-action>
            <v-icon color="grey darken-1">mdi-home</v-icon>
          </v-list-item-action>
          <v-list-item-title class="grey--text text--darken-1"></v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
      clipped-left
      color="red"
      dense
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-icon
        class="mx-4"
        large
      >
        mdi-youtube
      </v-icon>
      <v-toolbar-title class="mr-12 align-center">
        <span class="title">LLLin视频网站</span>
      </v-toolbar-title>
      <v-spacer />
      <v-row
        align="center"
        style="max-width: 650px"
      >
        <v-text-field
          :append-icon-cb="() => {}"
          placeholder="Search..."
          single-line
          append-icon="mdi-magnify"
          color="white"
          hide-details
        />
      </v-row>
    </v-app-bar>

    <v-content>
      <v-row
          justify="center"
          align="center"
        >
          <v-col >
           <nuxt-child/>
          </v-col>
        </v-row>
    </v-content>
  </v-app>
</template>

<script>

// 引入部分icon
 

    export default {
        props: {
            source: String, 
        },
        data: () => ({
            drawer: null,
            items: [{
                icon: 'mdi-home',
                text: '首页',
                link: '/'
            }, {
                icon: 'mdi-trending-up',
                text: '热门评论',
                link: '/HotComment'
            }, {
                icon: 'mdi-youtube-subscription',
                text: '热门视频',
                link: '/HotVideo'
            }, {
                icon: 'mdi-history',
                text: '观看历史'
            }, {
                icon: 'mdi-playlist-play',
                text: '视频列表'
            }, {
                icon: 'mdi-clock',
                text: '最近观看'
            }, ],
            items2: [{
                icon: 'mdi-account',
                text: '个人中心',
                link: '/profile'
            },{
                icon: 'mdi-message-text',
                text: '消息中心',
                link: '/HotComment'
            }, {
                icon: 'mdi-domain',
                text: '积分商城',
                link: '/HotComment'
            }],
        }),
        created() {
            this.$vuetify.theme.dark = true
        },
    }
</script>