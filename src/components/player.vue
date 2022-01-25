<template>
  <p>Now playing...</p>
  <p class="title">{{ song.name }}</p>
  <div>
    <div class="song-input-container">
      <input v-model="song.id" id="songId" @keypress="setSong($event)" type="text" placeholder="id, start seconds">
      <button @click="setRandom">Random</button>
    </div>

    <label for="songId" class="info">ENTER to save and play.
      <span class="strong">id</span> : what goes after "v=" in YouTube link.
    </label>
  </div>

  <div id="player">
    <button v-if="!playing" @click="playing = !playing;play()" id="">Play</button>
    <button v-if="playing" @click="playing = !playing;pause()">Pause</button>
  </div>
  <div id="fake-player" class="fake-player"></div>
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
        id: "5qap5aO4i9A"
      }
    }
  },
  props: {

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
          }
        }
      });
    },
    async setSong(event, element)  {
      if (event.key === "Enter") {
        this.playing = true;
        this.playerObj.loadVideoById(this.song.id);
      }
    },
    setRandom() {
      this.song.id = this.songs[parseInt(Math.random() * this.songs.length)];
    }
  },
  mounted() {
    this.setRandom();
    this.onYouTubePlayerAPIReady(this);
  }
}
</script>

<style>
  .fake-player {
    position: absolute;
    top: -1500px;
    right: 0px;
  }

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
  }

  .song-input-container {
    display: flex;
    justify-content: space-between;
  }

</style>

