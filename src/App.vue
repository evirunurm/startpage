<template>
<div class="background">
  <div class="container">
    <section class="top">
      <article class="left">
        <div class="today-info">
          <h1 class="date font--light">Monday, 11th Oct 2021</h1>
          <div class="clock">
            <h2>17:35</h2>
          </div>
        </div>
        <p class="fact font--light">A million words would not bring you back, I know because I tried, neither would a million tears, I know because I cried.</p>
        <button @click="updateFact();" type="button" name="button">Click</button>
      </article>
      <article class="right">
        <div class="img-container">
          <img class="img" :src="selectedImage.url" alt="">
        </div>
      </article>
    </section>
    <hr />
    <section class="bottom">
      <links-container />
      <links-container />
      <links-container />
      <links-container />
    </section>
  </div>
  <div class="settings-container">
    <div class="settings-button">
      <img src="./assets/settings.png" alt="">
    </div>
    <div class="settings-content">
      <div class="facts-settings">
        <p>Facts :</p>
        <div v-for="fact in facts" class="facts-settings__fact">
          <input @input="updateFact(fact);" :id="fact.id" type="radio" name="fact" :value="fact.id">
          <label :for="fact.id">{{ fact.name }}</label>
        </div>

      </div>
      <div class="image-settings-container">
        <div v-for="image in images" class="image-settings">
          <div @click="updateImage(image);" class="image-settings-left">
          </div>
          <div class="image-settings-name">
            <p class="image-name">{{ image.name }}</p>
          </div>
          <div @click="updateImage(image);" class="image-settings-right">
          </div>
        </div>
      </div>
      <p class="credits">Developed by Evelin Virunurm</p>
    </div>
  </div>
</div>
</template>

<script >
import LinksContainer from "./components/links_container.vue";

export default {
  data() {
    return {
      selectedImage: {
        name: "Street",
        url: "/images/background01.png",
      },
      selectedFacts: {
        name: "Cats",
        id: "cats",
        fact: String,
      },
      facts: [{
        name: "Cats",
        id: "cats",
        off: false,
        fetchURL: "https://cat-fact.herokuapp.com/facts",
      }, {
        name: "Dogs",
        id: "dogs",
        off: false,
        fetchURL: "https://dog-facts-api.herokuapp.com/api/v1/resources/dogs?number=1",
      }, {
        name: "Axolotls",
        id: "axolotls",
        off: false,
        fetchURL: "https://axoltlapi.herokuapp.com/",
      }, {
        name: "Anime Quotes",
        id: "anime_quotes",
        off: false,
        fetchURL: "https://animechan.vercel.app/api/random",
      }, {
        name: "General Facts (max. 5/h)",
        id: "general_facts",
        off: false,
        fetchURL: "https://api.fungenerators.com/fact/random",
      }, {
        name: "Off",
        id: "off",
        off: true,
      }],
      images: [{
        name: "Retro",
        url: "/images/background01.png",
        selected: true,
      }, {
        name: "Forest",
        url: "/images/background02.png",
        selected: Boolean,
      }, {
        name: "Puffy",
        url: "/images/background03.png",
        selected: Boolean,
      }, {
        name: "Night Moon",
        url: "/images/background04.png",
        selected: Boolean,
      }, {
        name: "Sunset Moon",
        url: "/images/background05.png",
        selected: Boolean,
      }, {
        name: "Street",
        url: "/images/background06.png",
        selected: Boolean,
      }, {
        name: "Coming Home",
        url: "/images/background07.png",
        selected: Boolean,
      }, {
        name: "Traveling Home",
        url: "/images/background08.png",
        selected: Boolean,
      }],
    }
  },
  methods: {
    updateImage(image) {
      this.selectedImage.url = image.url;
      this.selectedImage.name = image.name;
    },
    updateFact(facts) {
      this.selectedFacts.name = facts.name;
      this.selectedFacts.id = facts.id;
      this.selectedFacts.fact = this.fetchFact(facts.fetchURL);
      console.log(this.selectedFacts)
    },
    async fetchFact(url) {
      try {
        const response = await fetch(url, {
          mode: "cors"
        });
        console.log(response);
      }
      catch (e) {
        console.log(e);
      }

      return "";
    }
  },
  components: {
    LinksContainer,
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+Display:wght@100;200;300;400;500;600&display=swap');


:root {
  --grey: #2C2C2C;
  --pink: #FF90CC;
  --white: rgba(255, 255, 255, 0.5);
}

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: 'Noto Sans Display', sans-serif;
}

body {
  background-color: var(--grey);
  color: var(--white);
  display: flex;
}

.font--light {
  font-weight: 400;
}

.background {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  width: 70%;
  height: 60%;
}

.top {
  display: flex;
  width: 40em;
  justify-content: center;
  gap: 3em;
}

.left {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  text-align: right;
}

.right {
  display: flex;
  align-items: flex-start;
}

.img-container {
  max-width: 250px;
  max-height: 250px;
  overflow: hidden;
  border: 1px solid var(--white);
}

.img {
  width: 250px;
  height: 250px;
  object-fit: cover;
}

.bottom {
  display: flex;
  justify-content: space-between;
  width: 100%;
  text-align: center;
}

.clock h2 {
  font-size: 3em;
  font-weight: 500;
  color: white;
}

hr {
  width: 100%;
}

.image-settings {
  display: flex;
  align-items: center;
  gap: 10px;
}

.image-settings-left,
.image-settings-right {
  width: 10px;
  height: 10px;
  background-color: red;
}

.image-settings-right {
  background-color: green;
}
</style>
