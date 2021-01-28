<template lang="">
  <div>
    <v-toolbar-title>
      {{ title }}
      <v-btn
        color="success"
        icon
        small
        @click="openDialog"
        v-if="$store.state.fireUser"
      >
        <v-icon>mdi-pencil</v-icon>
      </v-btn>
    </v-toolbar-title>
    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <v-card-title primary-title>
          <span>제목 수정</span>
          <v-spacer />
          <v-btn color="success" icon @click="save">
            <v-icon>mdi-content-save</v-icon>
          </v-btn>
        </v-card-title>
        <v-card-text>
          <v-text-field label="제목" v-model="text" />
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
export default {
  props: ['title'],
  data() {
    return {
      dialog: false,
      text: this.title
    }
  },
  methods: {
    openDialog() {
      this.dialog = true
      this.text = this.title
    },
    save() {
      this.$firebase
        .database()
        .ref('site')
        .update({
          title: this.text
        })
      this.dialog = false
    }
  }
}
</script>
