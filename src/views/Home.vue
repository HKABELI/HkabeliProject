<template >
<div class="all">
    <div class="songs">
    <div class="animation"></div>
    <div v-for="(song, index) in songs" :key="song.name">
     <div @click="playPause(index)">
        <block    
        :name="song.name"
        :file="song.file"
        :playing="song.playing"
        :photo="song.photo"
      />
      </div>
    </div>
  </div>
    <div class="buttons">
      <div class="but" id="On" @click="PlayAll">
        <img src="../assets/play.svg" alt="" />
      </div>
      <div class="but" id="Off" @click="PauseAll">
        <img src="../assets/pause.svg" alt="" />
      </div>
    </div>
  </div>
</template>

<script>
import Block from "@/components/Block.vue";
//mp3 files
import song1 from "../assets/1.mp3";
import song2 from "../assets/2.mp3";
import song3 from "../assets/3.mp3";
import song4 from "../assets/4.mp3";
import song5 from "../assets/5.mp3";
import song6 from "../assets/6.mp3";
import song7 from "../assets/7.mp3";
import song8 from "../assets/8.mp3";
import song9 from "../assets/9.mp3";

//icons
import p1 from "../assets/icons/1.svg";
import p2 from "../assets/icons/2.svg";
import p3 from "../assets/icons/3.svg";
import p4 from "../assets/icons/4.svg";
import p5 from "../assets/icons/5.svg";
import p6 from "../assets/icons/6.svg";
import p7 from "../assets/icons/7.svg";
import p8 from "../assets/icons/8.svg";
import p9 from "../assets/icons/9.svg";



export default {
  //data of a component is it's attrivutes (the data it owns)
  data() {
    return {
      //array of song pads
      songs: [
        { name: "1", file: new Audio(song1), playing: false, photo: p1 },
        { name: "2", file: new Audio(song2), playing: false, photo: p2 },
        { name: "3", file: new Audio(song3), playing: false, photo: p3 },
        { name: "4", file: new Audio(song4), playing: false, photo: p4 },
        { name: "5", file: new Audio(song5), playing: false, photo: p5 },
        { name: "6", file: new Audio(song6), playing: false, photo: p6 },
        { name: "7", file: new Audio(song7), playing: false, photo: p7 },
        { name: "8", file: new Audio(song8), playing: false, photo: p8 },
        { name: "9", file: new Audio(song9), playing: false, photo: p9 },
      ],
    };
  },
  //if we use a son component, state it
  components: {
    Block,
  },
  //vue methods are the functions
  methods: {
    playPause(index) {
      //when refering to the component's data or props outside of HTML tags - we MUST use "this"
      let state = this.songs[index].playing;
      this.songs[index].playing = !state;
      if (!state) {
        this.songs[index].file.play();
        this.songs[index].file.loop=true;
      } else this.songs[index].file.pause();
    },
    PlayAll() {
      this.songs.forEach((song) => {
        song.playing = true;
        song.file.play();
      });
    },
    PauseAll() {
      this.songs.forEach((song) => {
        song.playing = false;
        song.file.pause();
      });
    },
  },
};
</script>

<style scoped>
.all{
  padding-top: 80px;
  height: auto;
  background-color:cornsilk;
}
.songs {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.buttons {
  display: flex;
  justify-content: center;
  padding: 20px;
}

.but {
  padding: 30px;
  margin: 20px;
}
.but image {
  height: 10px;
  width: 10px;
}

.but:hover{
  opacity: 0.5;
  cursor: pointer;
}

.but:active{
  transform: scale(0.8);
}

</style>