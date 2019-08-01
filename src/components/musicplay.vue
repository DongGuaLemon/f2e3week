<template>
  <div class="musicplaybg">
    <div class="title">
      <span class="songname">{{album[index].name}}</span>
      <span class="songer">{{album[index].songer}}</span>
      <audio
        id="audio"
        :src="src"
        ref="mmAudio"
        @timeupdate="currentTime = $event.target.currentTime"
        @ended="end($event)"
        @durationchange="getduration"
      ></audio>
      <div class="timebar">
        <div>
          <progressbar :percent="percent"></progressbar>
        </div>
        <div>
          <span>{{durationformat}}</span>
        </div>
      </div>
      <div>
        <img src="../assets/btn_ShufflePlayback.svg" width="50px" @click="randommusic" />
        <img src="../assets/btn_Rewind.svg" width="50px" @click="backmusic" />
        <img :src="playbtn" width="70px" @click="playmusic" />
        <img src="../assets/btn_Fast.svg" width="50px" @click="nextmusic" />
        <img :src="loopbtn" width="50px" @click="loop" />
      </div>
    </div>
  </div>
</template>
<script>
import progressbar from "./progress";
import playbtn from "../assets/btn_Play.svg";
import pausebtn from "../assets/btn_pause.svg";
import Slowly from "../assets/Slowly_Until_We_Get_There.mp3";
import IceCream from "../assets/Ice_Cream.mp3";
import Chicken from "../assets/If_I_Had_a_Chicken.mp3";
import Ghost from "../assets/A_Ghost_Town.mp3";
import repeatall from "../assets/btn_RepeatAll.svg";
import repeatone from "../assets/btn_RepeatOne.svg";
export default {
  data() {
    return {
      audio: "",
      playbtn: playbtn,
      loopbtn: repeatall,
      album: [
        {
          name: "Slowly Until We Get There",
          songer: "Joey Pecoraro",
          src: Slowly
        },
        { name: "A Ghost Town", songer: "Quincas Moreira", src: Ghost },
        { name: "Ice Cream", songer: "Joey Pecoraro", src: IceCream },
        { name: "If I Had a Chicken", songer: "Kevin MacLeod", src: Chicken }
      ],
      index: 0,
      random: false,
      duration: 0,
      currentTime: 0,
      percent: 0,
      time: null,
      fixduration: 0
    };
  },
  methods: {
    playmusic() {
      if (this.duration == 0) {
        this.duration = this.$refs.mmAudio.duration;
      }
      if (this.$refs.mmAudio.paused == false) {
        this.playbtn = playbtn;
      } else {
        this.playbtn = pausebtn;
      }
      this.$nextTick(() => {
        this.$refs.mmAudio.paused
          ? this.$refs.mmAudio.play()
          : this.$refs.mmAudio.pause();
        this.subduration();
      });
    },
    nextmusic() {
      this.playbtn = pausebtn;
      let index = this.index + 1;
      if (index === this.album.length) {
        index = 0;
      }
      this.index = index;
      this.getduration();
      this.subduration();
      this.$nextTick(() => {
        this.$refs.mmAudio.play();
      });
    },
    backmusic() {
      this.playbtn = pausebtn;
      let index = this.index - 1;
      if (index < 0) {
        index = this.album.length - 1;
      }
      this.index = index;
      this.getduration();
      this.subduration();
      this.$nextTick(() => {
        this.$refs.mmAudio.play();
      });
    },
    loop() {
      this.$refs.mmAudio.currentTime = 0;
      if (this.$refs.mmAudio.loop == true) {
        this.$nextTick(() => {
          this.loopbtn = repeatall;
          this.$refs.mmAudio.loop = false;
        });
      } else {
        this.$nextTick(() => {
          this.loopbtn = repeatone;
          this.$refs.mmAudio.loop = true;
        });
      }
    },
    randommusic() {
      this.random = !this.random;
      let randoms = Math.floor(Math.random() * 4);
      this.index = index;
      this.$nextTick(() => this.$refs.mmAudio.play());
    },
    end(e) {
      console.log(e);
      if (this.random == false) {
        this.nextmusic();
      } else {
        this.randommusic();
      }
    },
    getduration() {
      if (this.$refs.mmAudio.duration) {
        this.duration = this.$refs.mmAudio.duration;
        this.fixduration = this.$refs.mmAudio.duration;
      }
    },
    subduration() {
      let vm = this;
      if (this.playbtn == playbtn) {
        clearInterval(this.time);
      } else {
        clearInterval(this.time);
        this.time = setInterval(() => {
          this.duration = this.duration - 1;
        }, 1000);
      }
    }
  },
  mounted() {
    // this.audio = new Audio(this.album[this.index].src);
    // console.log(this.audio)
    this.audio = this.$refs.mmAudio;
  },
  computed: {
    src() {
      return this.album[this.index].src;
    },
    durationformat() {
      if (this.duration == 0) {
        return " ";
      } else {
        let min = parseInt(this.duration / 60);
        let sec = (this.duration - min * 60).toFixed(0);
        if (sec < 10) {
          return min + ":" + "0" + sec;
        } else {
          return min + ":" + sec;
        }
      }
    }
  },
  watch: {
    end(old, news) {
      console.log(old, news);
    },
    currentTime(time) {
      if (Math.abs(time - this.$refs.mmAudio.currentTime) > 0.5) {
        this.$refs.mmAudio.currentTime = time;
        this.currentTime = time;
      }
      this.percent = (this.currentTime / this.fixduration).toFixed(3);
      if (this.percent >= 1) {
        this.percent = 0;
      }
      //console.log(this.fixduration,3)
      console.log(this.percent);
    },
    duration() {
      if (this.duration == 0) {
        clearInterval(this.time);
      }
    }
  },
  components: {
    progressbar
  }
};
</script>
<style>
.musicplaybg {
  margin-top: 40px;
  width: 500px;
  height: 360px;
  background: -webkit-linear-gradient(
    rgba(255, 255, 255, 0.1),
    rgba(255, 255, 255, 0.7),
    rgba(255, 255, 255, 0.9),
    white
  );
  background: -o-linear-gradient(
    rgba(255, 255, 255, 0.1),
    rgba(255, 255, 255, 0.7),
    rgba(255, 255, 255, 0.9),
    white
  );
  background: -moz-linear-gradient(
    rgba(255, 255, 255, 0.1),
    rgba(255, 255, 255, 0.7),
    rgba(255, 255, 255, 0.9),
    white
  );
  background: linear-gradient(
    rgba(255, 255, 255, 0.1),
    rgba(255, 255, 255, 0.7),
    rgba(255, 255, 255, 0.9),
    white
  );
  position: relative;
}
.title {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 60px;
}
.songname {
  color: #413d3d;
  font-size: 22px;
  font-weight: bold;
}
.songer {
  color: #413d3d;
  font-size: 16px;
  font-family: "Merriweather Sans", "Regular";
}
.timebar {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
