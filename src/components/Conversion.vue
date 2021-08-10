<template>
  <v-container>
    <v-row>
      <v-col id="chat" cols="6">
        <v-card
          id="chatbox"
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
          <v-timeline class="ma-4">
            <v-timeline-item
              v-for="timeLine in allTimeLines"
              :key="timeLine.id"
              class="text-left"
              hide-dot="hideDot"
              :left="timeLine.align == 'l' ? true : false"
              :right="timeLine.align == 'r' ? true : false"
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
                v-if="timeLine.align == 'r' && timeLine.sent"
                min-height="55px"
                :id="'content_' + timeLine.id"
              >
                {{ timeLine.content }}
              </v-alert>
              <v-textarea
                solo
                backgroundColor="#E5E4DF"
                counter
                v-if="timeLine.align == 'r' && !timeLine.sent"
                :id="'content_' + timeLine.id"
                :value = timeLine.content
              >
              </v-textarea>
            </v-timeline-item>
          </v-timeline>
        </v-card>
        <v-btn
          class="float-right mr-4"
          elevation="2"
          color="#CDE589"
          @click="play()"
        >
          Play
        </v-btn>
        <br><br>
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
            <h3 class="text-center mt-1">Quick Replies</h3>
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
              @click="addReply"
            >
              <v-icon dark> mdi-plus </v-icon>
            </v-btn>
          </v-col>
        </v-row>
        <v-card
          id="replybox"
          outlined
          class="overflow-y-auto overflow-x-hidden mt-2"
          height="345px"
        >
          <v-alert
            v-for="reply in allReplies"
            :key="reply.id"
            elevation="2"
            class="ma-4"
            color="#CDE589"
            rounded
            @dblclick="sendReply"
          >
            {{reply.content}}
          </v-alert>
        </v-card>
        <br>
        <v-row>
          <v-col cols="8">
            <h3 class="text-center mt-1">Points</h3>
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
              @click="addPoint"
            >
              <v-icon dark> mdi-plus </v-icon>
            </v-btn>
          </v-col>
        </v-row>
        <v-card
          id="pointbox"
          outlined
          class="overflow-y-auto overflow-x-hidden mt-2"
          height="350px"
        >
          <v-alert
            v-for="point in allPoints"
            :key="point.id"
            elevation="2"
            class="ma-4"
            color="#E5E4DF"
            rounded
            @dblclick="sendPoint"
          >
            {{point.content}}
          </v-alert>
        </v-card>
      </v-col>
      <v-spacer></v-spacer>
      <v-col id="others" cols="3">
        <h3 class="text-center">Logger</h3>
        <v-alert
          outlined
          class="overflow-y-auto overflow-x-hidden mt-2"
          height="350px"
        >
          Log for wizords
        </v-alert>
        <br />
        <h3 class="text-center">WhiteBoard</h3>
        <v-textarea
          label="Notes"
          v-model="logger_message"
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

export default {
  name: "Conversation",
  components: {},
  data() {
    return {
      message: "",
      allTimeLines: [],
      allReplies: [],
      allPoints: [],
    };
  },
  created:function(){
    if (this.allTimeLines.length == 0) {
      this.allTimeLines.push({
        id: this.allTimeLines.length
          ? this.allTimeLines[this.allTimeLines.length - 1].id + 1
          : 0,
        content: "",
        align: "r",
        sent: false
      });
    }
    this.allReplies.push({
      content: "Okay, I got it.",
    });
    this.allReplies.push({
      content: "Sorry, please speak again.",
    });
    this.allPoints.push({
      id: this.allPoints.length
        ? this.allPoints[this.allPoints.length - 1].id + 1
        : 0, 
      content: "Point 1",
    });
    this.allPoints.push({
      id: this.allPoints.length
        ? this.allPoints[this.allPoints.length - 1].id + 1
        : 0, 
      content: "Point 2",
    });    
  },
  methods: {
    scrollToChatBoxBottom() {
      const container = this.$el.querySelector("#chatbox");
      container.scrollTop = container.scrollHeight;
    },
    scrollToReplyBoxBottom() {
      const container = this.$el.querySelector("#replybox");
      container.scrollTop = container.scrollHeight;
    },
    scrollToPointBoxBottom() {
      const container = this.$el.querySelector("#pointbox");
      container.scrollTop = container.scrollHeight;
    },
    addReply() {
      var word = prompt("Add a new quick reply:");
      if (word) {
        this.allReplies.push({
          content: word,
        });
      }
    },
    addPoint() {
      var word = prompt("Add a new point:");
      if (word) {
        this.allPoints.push({
          id: this.allPoints.length
            ? this.allPoints[this.allPoints.length - 1].id + 1
            : 0, 
          content: word,
        });
      }
    },
    sendReply(event) {
      var el = (event.target || event.srcElement)
      var last = this.allTimeLines[this.allTimeLines.length - 1];
      this.updateInput(last.id);
      if (
        this.allTimeLines.length &&
        last.align == "r" &&
        last.content == ""
      ) {
        this.allTimeLines.pop();
      }
      var index = this.allTimeLines.length ? this.allTimeLines.length - 1 : 0;
      while (index > 0 && !this.allTimeLines[index].sent) {
        this.allTimeLines[index].id += 1;
        index -= 1;
      }
      var newTimeLine = {
        id: this.allTimeLines.length
          ? index + 1
          : 0,
        content: el.textContent,
        align: "r",
        sent: true
      };
      this.allTimeLines.splice(index + 1, 0, newTimeLine);
    },
    sendPoint(event) {
      var el = (event.target || event.srcElement)
      var last = this.allTimeLines[this.allTimeLines.length - 1];
      this.updateInput(last.id);
      if (
        this.allTimeLines.length &&
        last.align == "r" &&
        last.content == ""
      ) {
        this.allTimeLines.pop();
      }
      var index = this.allTimeLines.length ? this.allTimeLines.length - 1 : 0;
      while (index > 0 && !this.allTimeLines[index].sent) {
        this.allTimeLines[index].id += 1;
        index -= 1;
      }
      var newTimeLine = {
        id: this.allTimeLines.length
          ? index + 1
          : 0,
        content: el.textContent,
        align: "r",
        sent: true
      };
      this.allTimeLines.splice(index + 1, 0, newTimeLine);
      this.deletePoint(el.textContent);
    },
    deletePoint(msg) {
      for (var i = 0; i < this.allPoints.length; i++) {
        if (this.allPoints[i].content.trim() == msg.trim()) {
          this.allPoints.splice(i, 1);
          this.updatePointsID();
          break;
        }
      }
    },
    updatePointsID() {
      for (var i = 0; i < this.allPoints.length; i++) {
        this.allPoints[i].id = i;
      }
    },
    updateInput(id) {
      if (!this.allTimeLines[id].sent) {
        this.allTimeLines[id].content = document.getElementById(`content_${this.allTimeLines[this.allTimeLines.length - 1].id}`).value;
      }
    },
    sendMsg() {},
    sendUserMsg() {
      if (this.message != "") {
        this.updateInput(this.allTimeLines.length - 1);
        if (
          this.allTimeLines.length &&
          this.allTimeLines[this.allTimeLines.length - 1].align == "r" &&
          this.allTimeLines[this.allTimeLines.length - 1].content == ""
        ) {
          this.allTimeLines.pop();
        }
        var index = this.allTimeLines.length ? this.allTimeLines.length - 1 : 0;
        while (index > 0 && !this.allTimeLines[index].sent) {
          this.allTimeLines[index].id += 1;
          index -= 1;
        }
        var newTimeLine = {
          id: this.allTimeLines.length
            ? index + 1
            : 0,
          content: this.message,
          align: "l",
          sent: true
        };
        this.allTimeLines.splice(index + 1, 0, newTimeLine);
        this.message = "";
      }
    },
    play() {
      var last = this.allTimeLines[this.allTimeLines.length - 1];
      if (last.align == "r" && !last.sent) {
        this.updateInput(last.id);
        last.sent = true;
      }
    },
  },
  watch: {
    allTimeLines: {
      handler() {
        if (this.allTimeLines[this.allTimeLines.length - 1].sent) {
          this.allTimeLines.push({
            id: this.allTimeLines.length
              ? this.allTimeLines[this.allTimeLines.length - 1].id + 1
              : 0,
            content: "",
            align: "r",
            sent: false
          });
        }
        this.scrollToChatBoxBottom();
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