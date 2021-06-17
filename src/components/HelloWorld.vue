<template>
  <v-container fluid>
    <v-row justify="center">
      <v-subheader>Today</v-subheader>
      <v-col v-for="event in eventList" :key="event.id" cols="12">
        <v-card dark>
          <v-card-title>{{ event.title }}</v-card-title>
          <v-card-text style="white-space: pre-wrap; word-wrap: break-word">{{
            event.body
          }}</v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn text color="secondary"> Cancel </v-btn>
            <v-btn text color="primary" @click="writeToFirestore"> Save </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { fireDb } from "@/plugins/firebase.js";
export default {
  name: "HelloWorld",
  data: () => ({
    eventList: [],
  }),
  async created() {
    const ref = fireDb.collection("dev").doc("event");
    let snap;
    try {
      snap = await ref.get();
    } catch (e) {
      console.error(e);
    }
    this.eventList = snap.data().eventList;
  },
  methods: {
    async writeToFirestore() {
      const ref = fireDb.collection("dev").doc("event");

      const document = {
        eventList: [
          {
            id: 1,
            title: "第二回モブプロ",
            body: "新型コロナウイルス対策を厚生労働省に助言する専門家組織は16日、会合を開いた。\n新規感染者数は全国的に減少傾向にあるとしたうえで、東京都内では人の流れが増え続けているなどとし、感染再拡大への警戒心を強めている。",
            dateTime: "2010-01-01 10:00",
            href: "https://github.com/vuetifyjs/vuetify-loader",
          },
          {
            id: 2,
            title: "第３回モブプロ",
            body: "新型コロナウイルス対策を厚生労働省に助言する専門家組織は16日、会合を開いた。新規感染者数は全国的に減少傾向にあるとしたうえで、東京都内では人の流れが増え続けているなどとし、感染再拡大への警戒心を強めている。",
            dateTime: "2010-01-01 10:00",
            href: "https://github.com/vuetifyjs/vuetify-loader",
          },
        ],
      };
      try {
        await ref.set(document);
      } catch (e) {
        console.error(e);
      }
    },
  },
};
</script>
