<template>
  <div>
    <v-card>
      <v-card-title primary-title>
        <div class="caption">게시판 쓰고 읽기</div>
      </v-card-title>
      <v-card-actions>
        <v-btn text color="primary" @click="write">쓰기</v-btn>
        <v-btn text color="primary" @click="read">읽기</v-btn>
      </v-card-actions>
    </v-card>

    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <form>
          <v-card-text>
            <v-text-field label="title" v-model="items.title"></v-text-field>
            <v-text-field
              label="content"
              v-model="items.content"
            ></v-text-field>
          </v-card-text>
          <v-card-actions>
            <v-btn text color="success" @click="save">저장하기</v-btn>
          </v-card-actions>
        </form>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
export default {
  data() {
    return {
      dialog: false,
      items: {
        title: '',
        content: ''
      },
      data: []
    }
  },
  methods: {
    write() {
      this.dialog = true
      console.log(this.$firebase.firestore())
    },

    async read() {
      const docRef = await this.$firebase
        .firestore()
        .collection('boards')
        .get()

      docRef.docs.forEach(doc => {
        console.log(doc.id)
        console.log(doc.data())
      })

      this.data = docRef.docs.map(doc => {
        const item = doc.data()
        return {
          id: doc.id,
          title: item.title,
          content: item.content
        }
      })

      console.log(this.data)
    },

    save() {
      this.$firebase
        .firestore()
        .collection('boards')
        .add(this.items)
      this.dialog = false
    }
  }
}
</script>
