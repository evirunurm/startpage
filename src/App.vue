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
        <p class="fact font--light"> {{selectedFactsType.fact}} </p>
        <button @click="updateFact(selectedFactsType);" type="button" name="button">Click</button>
      </article>
      <article class="right">
        <div class="img-container">
          <img class="img" :src="selectedImage.url" alt="">
        </div>
      </article>
    </section>
    <section class="bottom">
      <links-container v-for="shortcut in shortcuts" :name=shortcut.name :links=shortcut.links />
    </section>
  </div>
  <div class="settings-container">
    <div class="settings-button"  @click="isOptionsOpen = !isOptionsOpen">
      <img src="./assets/settings.png" alt="">
    </div>
    <div class="settings-content" v-if="isOptionsOpen">
      <div class="facts-settings">
        <p>Facts :</p>
        <div v-for="fact in factsTypes" class="facts-settings__fact">
          <input @input="updateFact(fact);saveDataToLocal();" :id="fact.id" type="radio" name="fact" :value="fact.id" :checked="fact.id == selectedFactsType.id">
          <label :for="fact.id">{{ fact.name }}</label>
        </div>
      </div>
      <div class="image-settings-container">
        <div class="image-settings" >
          <div @click="" class="image-settings-left">
          </div>
            <div class="image-settings-names">
                <div class="image-settings-name">
                  <p v-for="image in images" class="image-name">{{ image.name }}</p>
                </div>
            </div>

          <div @click="updateImage(image);saveDataToLocal();" class="image-settings-right">
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
      selectedFactsType: {
        url: "",
        fact: "",
        id: "off",
      },
      factsTypes: [
        {
          name: "Cats",
          id: "cats",
          url: "https://catfact.ninja/fact",
        },
        {
          name: "Dogs",
          id: "dogs",
          url: "http://dog-api.kinduff.com/api/facts",
        },
        {
          name: "Axolotls",
          id: "axolotls",
          url: "https://axoltlapi.herokuapp.com/",
        },
        {
          name: "Anime Quotes",
          id: "anime_quotes",
          url: "https://animechan.vercel.app/api/random",
        },
        {
          name: "Off",
          id: "off",
        }
      ],
      images: [
        {
          name: "Retro",
          url: "/images/background01.png",
          selected: true,
        },
        {
          name: "Forest",
          url: "/images/background02.png",
          selected: Boolean,
      },
        {
          name: "Puffy",
          url: "/images/background03.png",
          selected: Boolean,
        },
        {
          name: "Night Moon",
          url: "/images/background04.png",
          selected: Boolean,
        },
        {
          name: "Sunset Moon",
          url: "/images/background05.png",
          selected: Boolean,
        },
        {
          name: "Street",
          url: "/images/background06.png",
          selected: Boolean,
        },
        {
          name: "Coming Home",
          url: "/images/background07.png",
          selected: Boolean,
        },
        {
          name: "Traveling Home",
          url: "/images/background08.png",
          selected: Boolean,
        }
      ],
      isOptionsOpen: false,
      shortcuts: [
        {
          name: "general",
          links: [
            {
              name: "discord",
              link: "https://discord.com/channels/@me"
            },{
              name: "whatsapp",
              link: "https://discord.com/channels/@me"
            },
          ]
        },
        {
          name: "general",
          links: [
            {
              name: "discord",
              link: "https://discord.com/channels/@me"
            },
          ]
        },
        {
          name: "general",
          links: [
            {
              name: "discord",
              link: "https://discord.com/channels/@me"
            },
          ]
        },
        {
          name: "general",
          links: [
            {
              name: "discord",
              link: "https://discord.com/channels/@me"
            },
          ]
        }
      ]
    }
  },
  methods: {
    updateImage(image) {
      this.selectedImage.url = image.url;
      this.selectedImage.name = image.name;
    },
    updateFact(factsType) {
      if (factsType?.id == "off") {
        this.selectedFactsType.fact = "";
        this.selectedFactsType.id = "off";
        return;
      } else {
        this.selectedFactsType.id = factsType.id;
        this.selectedFactsType.url = factsType.url;
        this.fetchFact(this.selectedFactsType.url).then((fact) => {
          this.selectedFactsType.fact = fact;
          this.saveDataToLocal();
        });
      }

    },
    fetchFact(url) {
      const response = new Promise(async (resolve, reject) => {
        let res = await fetch(`https://cors-anywhere.herokuapp.com/${url}`, {
          mode: "cors"
        });
        let json = await res.json();
        let fact;
        switch (this.selectedFactsType.id) {
          case "cats":
            fact = json.fact;
            break;
          case "dogs":
            fact = json.facts[0];
            break;
          case "axolotls":
            fact = json.facts;
            break;
          case "anime_quotes":
            fact = json.quote;
            break;
          default:
            fact = "";
        }
        resolve(fact);
      });
      return response;
    },
    getLocalData() {
     if (localStorage.selectedImage) {
        this.selectedImage = JSON.parse(localStorage.getItem("selectedImage"));
      }
      if (localStorage.selectedFactsType) {
        this.selectedFactsType = JSON.parse(localStorage.getItem("selectedFactsType"));
      }
    },
    saveDataToLocal() {
      localStorage.setItem("selectedImage", JSON.stringify(this.selectedImage));
      localStorage.setItem("selectedFactsType", JSON.stringify(this.selectedFactsType));
    }
  },
  mounted() {
    this.getLocalData();
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

.image-settings-name p {
  background: pink;
  min-width: 100%;
}

.image-settings-name {
  background: red;
  display: flex;
/*  position: relative;
  left: -200%;*/
}

.image-settings-names {
  display: flex;
  overflow: hidden;
  width: 100px;
  background: green;
}

.settings-container {
  position: absolute;
  top: 1rem;
  right: 1rem;
  display: flex;
  flex-direction: column;
  align-items: end;
  gap: 0.5rem;
}



.settings-content {
  background: var(--grey);
  border: 1px solid var(--white);
  padding: 1rem;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}
</style>
