<template >
<div class="all">
    <div class="songs">
    <div class="animation"></div>
    <div v-for="(song, index) in songs" :key="song.name">
     <div @click="addRemoveSong(index)">
        <block    
        :name="song.name"
        :file="song.file"
        :padOn="song.padOn"
        :photo="song.photo"
      />
      </div>
    </div>
  </div>
    <div class="buttons">
      <div class="btn" id="On" @click="PlayAll">
        <img src="../assets/play.svg" alt="Play" />
      </div>
      <div class="btn" id="Off" @click="PauseAll">
        <img src="../assets/pause.svg" alt="Pause" />
      </div>
    </div>
  </div>
</template>

<script>
//import block component
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
        { name: "1", file: new Audio(song1), padOn: false, photo: p1 },
        { name: "2", file: new Audio(song2), padOn: false, photo: p2 },
        { name: "3", file: new Audio(song3), padOn: false, photo: p3 },
        { name: "4", file: new Audio(song4), padOn: false, photo: p4 },
        { name: "5", file: new Audio(song5), padOn: false, photo: p5 },
        { name: "6", file: new Audio(song6), padOn: false, photo: p6 },
        { name: "7", file: new Audio(song7), padOn: false, photo: p7 },
        { name: "8", file: new Audio(song8), padOn: false, photo: p8 },
        { name: "9", file: new Audio(song9), padOn: false, photo: p9 },
      ],
      markedToPlay: [],
      nextToPlay: [],
      IsLoopOn: false,
    };
  },
  components: {
    Block,
  },
  methods: {
    addRemoveSong(index) {                  
      let padState = this.songs[index].padOn; //turn from padOn to not padOn and vice versa
      this.songs[index].padOn = !padState;
      let song = this.songs[index];

      if (song.padOn) {                      //meaning the pad has been turned on
        if(this.IsLoopOn){
            this.nextToPlay.push(song);      //in case the loop is already running, push the song to a waiting list
            this.PlayAll();
        }
        else{                                //else, it shouldn't wait
            this.markedToPlay.push(song);
        }
      }
      else {
            setTimeout(()=> this.pausesong(index) ,(8-this.markedToPlay[0].file.currentTime) * 1000)
                   //meaning the pad has been turned off
            let toRemove = this.markedToPlay.indexOf(song);    //get the index of the song in the markedToPlay array
            if(toRemove > -1) 
                this.markedToPlay.splice(toRemove, 1);         //remove from array using splice

            if(this.markedToPlay.length < 1) 
                this.IsLoopOn = false;                         //if no more songs are playing, stop the loop

            toRemove = this.nextToPlay.indexOf(song);          //remove from waiting array aswell
            if(toRemove > -1)
                this.nextToPlay.splice(toRemove, 1);
                            //pause the music
        }
      
    },
    pausesong(index){
        
        this.songs[index].file.pause()
    },
    PlayAll() {
        
          if (this.markedToPlay.length > 0){       //meaning there are songs to be played
          this.IsLoopOn = true;                 //mark loop as active
          this.markedToPlay.forEach((song) => { //go through all songs to be played - play them in a loop and activate pads
            song.file.loop = true;  //loop
            song.padOn = true;      //pad turns green
            song.file.play();       //music playing
          });
          
          let timeRemaining = 8 - this.markedToPlay[0].file.currentTime; //calculate the time remaining for the currect music loop 
          if(this.nextToPlay.length > 0)   // check if there are songs to be joined
              setTimeout(this.playWaiting, timeRemaining * 1000); //wait until the current loop is over and call playWaiting
       }
    },
    PauseAll() {
        this.IsLoopOn = false;         //mark loop as inactive
        this.songs.forEach((song) => { //pause all songs
            song.file.pause();
        });
    },
    playWaiting() {
      this.nextToPlay.forEach(song => { //push all waiting songs to the markedToPlay array
        this.markedToPlay.push(song);
      })
      this.nextToPlay = []; //empty out the waiting list
      this.PlayAll(); //call the play all function again
    
    }
  },
};
</script>

<style scoped>
.all{
  padding-top: 80px;
  height: auto;
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

.btn {
  padding: 30px;
  margin: 20px;
}
.btn image {
  height: 10px;
  width: 10px;
}

.btn:hover{
  opacity: 0.5;
  cursor: pointer;
}

.btn:active{
  transform: scale(0.8);
}

</style>