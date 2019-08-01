<template>
  <div class="musicmain">
    <div class="photoframe"></div>
    <musicplay></musicplay>
  </div>
</template>
<script>
import musicplay from "./musicplay";
export default {
  components: {
    musicplay
  }
};
</script>
<style>
.musicmain {
  background-image: url("../assets/spring.png");
  /* background-image: url('https://scontent.fkhh1-1.fna.fbcdn.net/v/t1.0-9/18766026_1520121901365594_4842225143218472140_n.jpg?_nc_cat=101&_nc_ht=scontent.fkhh1-1.fna&oh=77b4490910edbd8122cd931aeba534ba&oe=5D2A0EB3'); */
  background-size: 500px 100%;
  background-repeat: no-repeat;
  background-position: center;
  min-height: 100%;
  width: 100%;
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}
.photoframe {
  width: 327px;
  height: 350px;
  border: 10px solid #ffffff;
  margin: 0 auto;
  margin-top: 30px;
}
</style>
