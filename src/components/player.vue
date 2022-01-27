<template>
  <p>Now playing...</p>
  <p class="title">{{ song.name }}</p>
  <div>
    <div class="song-input-container">
      <input v-model="song.id" id="songId" @keypress="setSong($event)" type="text" placeholder="ENTER to save and play.">
      <div id="player">
        <button v-if="!playing" @click="playing=!playing;play()">
          <svg width="60" height="61" viewBox="0 0 60 61" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M54.6892 23.3446L11.5777 1.78885C6.2585 -0.87075 0 2.99721 0 8.94427V52.0557C0 58.0028 6.25851 61.8708 11.5777 59.2111L54.6892 37.6554C60.5856 34.7072 60.5855 26.2928 54.6892 23.3446Z" fill="black"/>
          </svg>
        </button>
        <button v-if="playing" @click="playing=!playing;pause()">
          <svg width="57" height="61" viewBox="0 0 57 61" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M9 9L9 52" stroke="black" stroke-width="17" stroke-linecap="round"/>
            <path d="M48 9L48 52" stroke="black" stroke-width="17" stroke-linecap="round"/>
          </svg>
        </button>
      </div>

      <button class="random-button" @click="setRandom();setSong({key: 'Enter'})">
        Random
      </button>
    </div>
    <label for="songId" class="info">
      <span class="strong">id</span> : what goes after "v=" in YouTube link.
    </label>
    <div class="volume-container">
      <input id="volume" @change="playerObj.setVolume(song.volume);changeVolumeGradient();this.saveLocal();" v-model="song.volume" type="range" min="0" max="50" step="2.5">
    </div>
  </div>
<!--  style="position:absolute;top:-1500px; right:0px;"-->
  <div id="fake-player" >
  </div>
</template>

<script>
export default {
  name: "player",
  data() {
    let playerObj;
    return {
      songs : [
        "lTRiuFIWV54",
        "5qap5aO4i9A",
        "hisfbC_Ov1U",
      ],
      playing: false,
      song: {
        name: "",
        id: "5qap5aO4i9A",
        volume: ""
      }
    }
  },
  methods: {
    play() {
      this.playerObj.playVideo();
    },
    pause() {
      this.playerObj.pauseVideo();
    },
    onYouTubePlayerAPIReady(context) {
      this.playerObj = new YT.Player('fake-player', {
        videoId: this.song.id,
        loop: true,
        events: {
          onReady: function (e) {
            console.log("Player loaded successfully")
          },
          onStateChange: function (e) {
            context.song.name = e.target.playerInfo.videoData.title;
            e.target.setVolume(context.song.volume);
            context.saveLocal();
          }
        }
      });
    },
    setSong(event)  {
      if (event.key === "Enter") {
        this.playing = true;
        this.playerObj.loadVideoById(this.song.id);
      }
    },
    setRandom() {
      this.song.id = this.songs[parseInt(Math.random() * this.songs.length)];
    },
    saveLocal() {
      localStorage.setItem("song", JSON.stringify(this.song));
    },
    getLocal() {
      if (localStorage.song) {
        this.song = JSON.parse(localStorage.getItem("song"));
      } else {
        this.setRandom();
      }
    },
    changeVolumeGradient() {
      const el = document.getElementById('volume');
      el.value = this.song.volume;
      let value = (el.value - el.min) / (el.max - el.min) * 90;
      el.style.background = `linear-gradient(to right, var(--primaryColor) 0%, var(--primaryColor) ${value + 5}%, var(--secondaryColor) ${value + 5}%, var(--secondaryColor) 100%`;
    }
  },
  mounted() {
    this.getLocal();
    window.YT.ready( () => {
      console.log("yes")
      this.onYouTubePlayerAPIReady(this);
      this.changeVolumeGradient();
    })
  }
}
</script>

<style scoped>
  .strong {
    font-weight: bold;
    text-decoration: underline;
  }

  .info {
    font-size: 0.65em;
    opacity: 0.75;
  }

  .title {
    max-width: 300px;
    font-size: 0.9em;
  }

  .song-input-container {
    display: flex;
    justify-content: space-between;
    gap: 0.75em;
    align-items: center;
  }

  #player button {
    background: none;
    border: none;
    cursor: pointer;
  }

  button svg {
    width: 20px;
    height: 20px;
  }

  #player button {
    opacity: 25%;
  }

  #player button:hover {
    opacity: 100%;
  }

  .random-button {
    padding: 0.45em 0.8em;
    background: var(--secondaryColor);
    border: none;
    cursor: pointer;
    color: var(--primaryColor);
  }

  .random-button:hover {
    background: var(--primaryColor);
    color: var(--secondaryColor);
  }

  input {
    padding: 0.25em 0.5em;
    background-color: var(--backgroundColor);
    color: var(--fontColor);
    border: 2px solid var(--fontColor);
    outline: none;
  }

  input:focus {
    color: var(--primaryColor);
    background: var(--secondaryColor);
  }

  .volume-container {
    width: 100%;
  }

  #volume {
    margin: 0.85em 0;
    -webkit-appearance: none;
    appearance: none;
    background: var(--secondaryColor);
    width: 100%;
    height: 5px;
    border-radius: 5px;
    -webkit-transition: .2s;
    border: none;
    cursor: pointer;
    outline: none;
    transition: background 450ms ease-in;
  }


  #volume::-webkit-slider-thumb {
    -webkit-appearance: none; /* Override default look */
    appearance: none;
    width: 18px;
    height: 18px;
    border-radius: 100%;
    background: var(--primaryColor);
    opacity: 0.6;
  }

  #volume::-webkit-slider-thumb:hover {
    opacity: 1;
  }

</style>

