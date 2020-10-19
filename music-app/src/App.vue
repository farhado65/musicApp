<template>
  <div id="app" :style="{background: 'url('+bg+') no-repeat center center'}">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      current: {},
      index: 0,
      bg: 'https://picsum.photos/id/100/1980/1080',
      isPlaying: false,
      songs: [
        {
          title: 'Passanger',
          artist: 'Passanger',
          src: require('./assets/Passenger - The Way That I Love You (Lyric Video).mp3'),
          background: 'https://picsum.photos/id/1/1980/1080'
          
        },{
          title: 'Grateful',
          artist: 'Neffex',
          src: require('./assets/neffex-grateful.mp3'),
          background: 'https://picsum.photos/id/2/1980/1080'
        },
        {
          title: 'Invincible',
          artist: 'Deaf Kev',
          src: require('./assets/deaf-kev-invincible.mp3'),
          background: 'https://picsum.photos/id/3/1980/1080'
        },
        
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined") {
        this.bg= 'https://picsum.photos/id/5/1980/1080';
        this.current = song;
        this.player.src = this.current.src;
      }
      console.log(song.background, this.bg);
      this.bg = song.background;

      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }

        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
}
</script>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
body {
	font-family: sans-serif;
  min-height: 100vh;
}
#app {
  height: 100vh;
}
header {
	display: flex;
	justify-content: center;
	align-items: center;
	padding: 15px;
	background-color: #332e2e;
	color: #FFF;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.song-title {
  color: #495e79;
  font-size: 32px;
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
  padding: 30px 15px;
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

.play, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #FFF;
  background-color: #CC2E5D;
}

.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #FFF;
  background-color: #51b3eb;
}

.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color:rgb(73, 106, 156);
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
  color:rgb(216, 66, 98);
}
.playlist .song:hover {
  color: #5b58ff;
}

.playlist .song.playing {
  color: #FFF;
  background-image: linear-gradient(to right, #462ecc, #FF5858);
}
</style>