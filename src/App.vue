<template>
  <v-app>
    <v-navigation-drawer app v-model="drawer">
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="title">
            Application
          </v-list-item-title>
          <v-list-item-subtitle>
            subtext
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <Menu :items="site.items" />
    </v-navigation-drawer>

    <v-app-bar app flat>
      <v-app-bar-nav-icon @click="drawer = !drawer" />
      <Title :title="site.title" />

      <v-spacer></v-spacer>
      <Sign />
    </v-app-bar>

    <v-main>
      <v-container>
        <router-view></router-view>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Menu from '@/views/site/Menu'
import Sign from '@/views/site/Sign'
import Title from '@/views/site/Title'
export default {
  name: 'App',
  components: { Menu, Sign, Title },

  data() {
    return {
      drawer: null,
      site: {
        title: '뷰 사이트 만들기',
        items: [
          {
            icon: 'mdi-home',
            items: [{ title: '소개', to: '/' }],
            title: '홈'
          },
          {
            icon: 'mdi-clock',
            active: true,
            items: [
              { title: '뷰', to: '/vue' },
              { title: '리엑트', to: '/react' },
              { title: '앵귤러', to: '/angular' }
            ],
            title: '자바스크립트'
          },
          {
            icon: 'mdi-account-multiple',
            items: [{ title: '최근 글', to: '/recent' }],
            title: '활동'
          }
        ]
      }
    }
  },
  created() {
    this.$firebase
      .database()
      .ref('site')
      .on('value', sn => {
        const v = sn.val()
        if (!v) {
          this.$firebase
            .database()
            .ref('site')
            .set(this.site)
        } else {
          this.site = v
        }
      })
  },
  methods: {}
}
</script>
