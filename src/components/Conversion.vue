<template>
  <v-container>
    <v-row>
      <v-col id="chat" cols="6">
        <v-card
          elevation="0"
          class="overflow-y-auto overflow-x-hidden"
          max-height="800px"
        >
          <v-row>
            <v-col>
              <h3 class="text-center">User</h3>
            </v-col>
            <v-col>
              <h3 class="text-center">Wizord</h3>
            </v-col>
          </v-row>
          <v-timeline class="ma-4" reverse="reverse">
            <v-timeline-item
              v-for="timeLine in timeLines"
              :key="timeLine.id"
              class="text-left"
              hide-dot="hideDot"
              :left="timeLine.align == 'r' ? true : false"
              :right="timeLine.align == 'l' ? true : false"
            >
              <v-alert
                elevation="2"
                outlined
                rounded
                v-if="timeLine.align == 'l'"
              >
                {{ timeLine.content }}
              </v-alert>
              <v-alert
                elevation="2"
                color="#E5E4DF"
                rounded
                v-if="timeLine.align == 'r'"
                min-height="55px"
                :id="'content_' + timeLine.id"
              >
                <draggable
                  v-model="myArray"
                  group="quick_reply"
                  @start="drag = true"
                  @end="drag = false"
                >
                  {{ timeLine.content }}
                </draggable>
              </v-alert>
            </v-timeline-item>
          </v-timeline>
        </v-card>
        <h3 class="text-center">User (for test)</h3>
        <v-textarea
          label="Message"
          v-model="message"
          outlined
          no-resize
          counter
          class="overflow-y-auto overflow-x-hidden pt-1"
          height="350px"
        >
        </v-textarea>
        <v-btn
          class="ml-4 mr-2"
          elevation="2"
          color="#CDE589"
          @click="sendUserMsg()"
        >
          Send
        </v-btn>
      </v-col>
      <v-spacer></v-spacer>
      <v-col id="box" cols="3">
        <v-row>
          <v-col cols="8">
            <h3 class="text-center mt-1">Conversion box</h3>
          </v-col>
          <v-spacer></v-spacer>
          <v-col>
            <v-btn
              class="fab mx-2"
              icon
              outlined
              dark
              x-samll
              color="#000000"
              @click="addCount"
            >
              <v-icon dark> mdi-plus </v-icon>
            </v-btn>
          </v-col>
        </v-row>
        <v-card
          outlined
          class="overflow-y-auto overflow-x-hidden mt-2"
          max-height="800px"
        >
          <draggable
            v-model="myArray"
            :group="{ name: 'quick_reply', pull: 'clone', put: false }"
            :clone="cloneDog"
            @start="drag = true"
            @end="drag = false"
          >
            <v-alert elevation="2" class="ma-4" color="#CDE589" rounded>
              Okay, I got it.
            </v-alert>
            <v-alert elevation="2" class="ma-4" color="#CDE589" rounded>
              Sorry, please speak again.
            </v-alert>
          </draggable>
          <draggable
            v-model="myArray"
            :group="{ name: 'quick_reply' }"
            @start="drag = true"
            @end="drag = false"
          >
            <v-text-field
              v-for="n in itemcount"
              :key="n"
              clearable
              solo
              class="ml-4 mr-4 mt-4 mb-n7"
              filled
              background-color="#CDE589"
            >
            </v-text-field>
          </draggable>
          <draggable
            v-model="myArray"
            :group="{ name: 'quick_reply', pull: 'clone', put: false }"
            :clone="cloneDog"
            @start="drag = true"
            @end="drag = false"
          >
            <v-alert elevation="2" class="ma-4" color="#E5E4DF" rounded>
              Point 1
            </v-alert>
            <v-alert elevation="2" class="ma-4" color="#E5E4DF" rounded>
              Point 2
            </v-alert>
          </draggable>
        </v-card>
      </v-col>
      <v-spacer></v-spacer>
      <v-col id="others" cols="3">
        <h3 class="text-center">Logger</h3>
        <v-alert
          outlined
          class="overflow-y-auto overflow-x-hidden mt-2"
          height="350"
        >
          Log for wizords
        </v-alert>
        <br />
        <h3 class="text-center">WhiteBoard</h3>
        <v-textarea
          label="Notes"
          v-model="message"
          outlined
          no-resize
          counter
          class="overflow-y-auto overflow-x-hidden pt-1"
          height="350px"
        >
        </v-textarea>
        <v-btn class="ml-2 mr-4" elevation="2" color="#CDE589"> Save </v-btn>
        <v-btn
          class="ml-4 mr-2"
          elevation="2"
          color="#CDE589"
          @click="sendMsg()"
        >
          Send
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import draggable from "vuedraggable";

var itemcount = 0;

export default {
  name: "Conversation",
  components: {
    draggable,
  },
  data() {
    return {
      itemcount,
      message: "",
      timeLines: [],
    };
  },
  methods: {
    addCount() {
      this.itemcount += 1;
    },
    sendMsg() {
      if (this.message != "") {
        if (
          this.timeLines.length &&
          this.timeLines[this.timeLines.length - 1].align == "r" &&
          this.timeLines[this.timeLines.length - 1].content == ""
        ) {
          this.timeLines.pop();
        }
        this.timeLines.push({
          id: this.timeLines.length
            ? this.timeLines[this.timeLines.length - 1].id + 1
            : 0,
          content: this.message,
          align: "r",
          order: this.timeLines.length + 1,
        });
        this.message = "";
      }
    },
    sendUserMsg() {
      if (this.message != "") {
        if (
          this.timeLines.length &&
          this.timeLines[this.timeLines.length - 1].align == "r" &&
          !document
            .getElementById(
              `content_${this.timeLines[this.timeLines.length - 1].id}`
            )
            .firstElementChild.firstElementChild.hasChildNodes() &&
          this.timeLines[this.timeLines.length - 1].content == ""
        ) {
          this.timeLines.pop();
        }
        this.timeLines.push({
          id: this.timeLines.length
            ? this.timeLines[this.timeLines.length - 1].id + 1
            : 0,
          content: this.message,
          align: "l",
          order: this.timeLines.length + 1,
        });
        this.message = "";
      }
    },
  },
  watch: {
    timeLines: {
      handler() {
        if (this.timeLines[this.timeLines.length - 1].align == "l") {
          this.timeLines.push({
            id: this.timeLines.length
              ? this.timeLines[this.timeLines.length - 1].id + 1
              : 0,
            content: "",
            align: "r",
            order: this.timeLines.length + 1,
          });
        }
      },
      deep: true,
    },
  },
};
</script>

<style>
#user_input {
  width: 100px;
}
</style>
