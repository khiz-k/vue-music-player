<template>
  <div id="app">
    <header>
      <h1>Music Player</h1>
    </header>
    <section class="player">
      <h2 class="playing">Playing:</h2>
      <h2 class="song-title">
        {{ current.title }} -
        <span>{{ current.artist }}</span>
      </h2>
      <div class="controls">
        <button class="prev" @click="prev">Prev</button>
        <button class="play" v-if="!isPlaying" @click="play">Play</button>
        <button class="pause" v-else @click="pause">Pause</button>
        <button class="next" @click="next">Next</button>
      </div>
    </section>
    <section class="playlist">
      <h2>Playlist:</h2>
      <button
        v-for="song in songs"
        :key="song.src"
        @click="play(song)"
        :class="song.src == current.src ? 'song playing' : 'song'"
      >{{ song.title }} - {{ song.artist }}</button>
    </section>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "Donuts",
          artist: "2scoops",
          src: require("./assets/2scoops_-_donuts.mp3"),
        },
        {
          title: "A Different Way X Silence X Light",
          artist: "Marshmello Mashup",
          src: require("./assets/A Different Way X Silence X Light.mp3"),
        },
      ],
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;

        this.player.src = this.current.src;
      }

      this.player.play();
      this.player.addEventListener(
        "ended",
        function () {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }

          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  border-radius: 150px;
}
body {
  background-color: #323232;
  font-family: sans-serif;
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
}
header {
  display: flex;
  justify-content: center;
  color: white;
  margin: 25px auto;
  font-size: 40px;
}
.playing {
  text-decoration: underline;
  font-size: 36px;
  color: #212121;
}

.player {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 50px;
  background-color: plum;
}
.song-title {
  padding: 30px 0px;
  color: #53565a;
  font-size: 24px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
}

.controls {
  display: flex;
  justify-content: center;
  align-items: center;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.8;
}

.play,
.pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #fff;
  background-color: blue;
}

.next,
.prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #fff;
  background-color: lightblue;
}

.playlist {
  background-color: blueviolet;
  padding: 50px;
}
.playlist > h2 {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  color: #212121;
  text-decoration: underline;
  font-size: 36px;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .song:hover {
  color: lightblue;
}

.playlist .song.playing {
  color: #fff;
  background-image: linear-gradient(to right, blue, lightblue);
}
</style>
