<template lang="">
  <div>
    <v-menu offset-y v-if="!$store.state.fireUser">
      <template v-slot:activator="{ on }">
        <v-btn icon color="grey darken-1" v-on="on">
          <v-icon>mdi-account</v-icon>
        </v-btn>
      </template>
      <v-list>
        <v-list-item>
          <v-btn text @click="signInGoogle">
            <v-icon left>mdi-google</v-icon><span>구글 로그인</span>
          </v-btn>
        </v-list-item>
      </v-list>
    </v-menu>

    <v-menu offset-y v-else>
      <template v-slot:activator="{ on }">
        <v-btn icon color="grey darken-1" v-on="on">
          <v-avatar size="30">
            <img :src="$store.state.fireUser.photoURL" alt="alt" />
          </v-avatar>
        </v-btn>
      </template>
      <v-list>
        <v-list-item>
          <v-btn text block @click="logout">로그아웃</v-btn>
        </v-list-item>
      </v-list>
    </v-menu>
  </div>
</template>
<script>
export default {
  data() {
    return {}
  },
  methods: {
    signInGoogle() {
      var provider = new this.$firebase.auth.GoogleAuthProvider()
      this.$firebase.auth().languageCode = 'ko'
      this.$firebase.auth().signInWithPopup(provider)
    },
    logout() {
      this.$firebase.auth().signOut()
    }
  }
}
</script>
