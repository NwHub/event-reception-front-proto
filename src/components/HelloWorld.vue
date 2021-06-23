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
            <v-dialog v-model="dialog" persistent max-width="600px">
              <template v-slot:activator="{ on, attrs }">
                <v-btn color="primary" dark v-bind="attrs" v-on="on">
                  予約
                </v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="text-h5">参加予約</span>
                </v-card-title>
                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col cols="12">
                        <v-text-field label="氏名*" required></v-text-field>
                      </v-col>
                      <v-col cols="12">
                        <v-text-field label="Email*" required></v-text-field>
                      </v-col>
                    </v-row>
                  </v-container>
                  <small>*必須</small>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" text @click="dialog = false">
                    Close
                  </v-btn>
                  <v-btn color="blue darken-1" text @click="dialog = false">
                    Save
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
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
  data: () => ({ dialog: false, eventList: [] }),
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
            body:
              "新型コロナウイルス対策を厚生労働省に助言する専門家組織は16日、会合を開いた。\n新規感染者数は全国的に減少傾向にあるとしたうえで、東京都内では人の流れが増え続けているなどとし、感染再拡大への警戒心を強めている。",
            dateTime: "2010-01-01 10:00",
            href: "https://github.com/vuetifyjs/vuetify-loader",
          },
          {
            id: 2,
            title: "第３回モブプロ",
            body:
              "新型コロナウイルス対策を厚生労働省に助言する専門家組織は16日、会合を開いた。新規感染者数は全国的に減少傾向にあるとしたうえで、東京都内では人の流れが増え続けているなどとし、感染再拡大への警戒心を強めている。",
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
