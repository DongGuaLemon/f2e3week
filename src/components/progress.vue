<template>
  <div class="mmProgress" ref="mmProgress">
    <div class="mmProgress-bar" @click="updatecurrentTimes($event)"></div>
    <div
      class="mmProgress-inner"
      @click="backcurrentTimes($event)"
      :style="barWidth"
      ref="mmProgressInner"
    >
      <div class="mmProgress-dot"></div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    percent: {
      type: [String, Number],
      default: 0
    },
    fixduration: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      barWidth: "width:0px",
      timeprogress: null,
      timers: 0,
      dotoffset: 0,
      updatecurrentTime: 0,
      backcurrentTime: 0
    };
  },
  methods: {
    //progress狀態條
    progress() {
      this.timeprogress = setInterval(() => {
        this.dotoffset = 200 * this.percent;
        this.barWidth = "width:" + this.dotoffset + "px";
      }, 1000);
    },
    //點擊未來
    updatecurrentTimes(e) {
      this.updatecurrentTime = this.fixduration * (e.offsetX / 200);
      this.$emit("newcurrentTime", this.updatecurrentTime);
    },
    //點擊過去
    backcurrentTimes(e) {
      this.backcurrentTime = e.offsetX / 2;
      this.$emit("backcurrentTime", this.backcurrentTime);
    }
  },
  mounted() {
    this.progress();
  }
};
</script>
<style>
.mmProgress {
  position: relative;
  padding: 5px;
  user-select: none;
  cursor: pointer;
  overflow: hidden;
}
.mmProgress-bar {
  height: 2px;
  width: 200px;
  background: #dedede;
}

.mmProgress-inner {
  position: absolute;
  top: 50%;
  left: 5px;
  display: inline-block;
  height: 2px;
  margin-top: -1px;
  background: #413d3d;
}
.mmProgress-dot {
  position: absolute;
  top: 50%;
  right: -5px;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background-color: #413d3d;
  transform: translateY(-50%);
}
</style>