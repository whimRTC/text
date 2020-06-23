<template>
  <div>
    <template v-if="isMe">
      <textarea class="text-area" v-model="text"></textarea>
      <a class="btn blue" @click="send">送信</a>
    </template>
    <div class="text" v-if="textSent">
      <span>{{ textSent }}</span>
      <img :src="image_path(image)" alt="" />
    </div>
  </div>
</template>
<script>
const FUKIDASHI = ["01", "02", "03", "04", "14"];
export default {
  name: "Player",
  props: {
    displayUser: {
      // 表示されているUserの情報
      type: Object,
      required: true
    }
  },
  data() {
    return {
      text: this.displayUser.name
    };
  },
  computed: {
    isMe() {
      return this.displayUser.id === this.$whim.accessUser.id;
    },
    textSent() {
      return this.$whim.state[this.displayUser.id] || "";
    },
    image() {
      return (this.$whim.state.image || {})[this.displayUser.id];
    }
  },
  methods: {
    send() {
      const fukidashi = FUKIDASHI[Math.floor(Math.random() * FUKIDASHI.length)];
      this.$whim.assignState({
        [this.$whim.accessUser.id]: this.text,
        image: {
          [this.$whim.accessUser.id]: `fukidashi${fukidashi}.png`
        }
      });
    },
    image_path(slug) {
      return require("@/assets/" + slug);
    }
  }
};
</script>
<style lang="scss" scoped>
.text {
  position: absolute;
  top: 90%;
  left: 50%;
  transform: translate(-50%, -90%);
  color: #ffffff;
  font-size: 30px;
  font-family: "Noto Sans JP", sans-serif;
}
.text-area {
  font-size: 30px;
  font-family: "Noto Sans JP", sans-serif;
}
.btn {
  display: inline-block;
  padding: 0.3em 1em;
  // margin: 0px 5px;
  text-decoration: none;
  // height: 26px;

  border-radius: 3px;
  transition: 0.4s;

  &.blue {
    color: #67c5ff;
    border: solid 2px #67c5ff;
    &:hover {
      background: #67c5ff;
      color: white;
    }
  }
  &.red {
    color: #ff67a6;
    border: solid 2px #ff67a6;
    &:hover {
      background: #ff67a6;
      color: white;
    }
  }
}
span {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  z-index: 2;
  white-space: pre-line;
}
img {
  width: 50vmin;
  opacity: 0.8;
  transform: rotate(180deg);
}
</style>
