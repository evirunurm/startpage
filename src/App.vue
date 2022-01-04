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
      <links-container v-on:modifyShortcuts="modifyShortcuts" v-for="shortcut in shortcuts" :name=shortcut.name :links=shortcut.links :index="shortcuts.indexOf(shortcut)"/>
    </section>
  </div>
  <div class="settings-container" >
    <div class="settings-button"  @click="isOptionsOpen = !isOptionsOpen">
      <img src="./assets/settings.png" alt="">
    </div>
    <div class="settings-content" v-show="isOptionsOpen">
      <div class="facts-settings">
        <p>Facts :</p>
        <div  v-for="fact in factsTypes" class="facts-settings__fact">
          <input @input="updateFact(fact);saveDataToLocal();" :id="fact.id" type="radio" name="fact" :value="fact.id" :checked="fact.id == selectedFactsType.id">
          <label :for="fact.id">{{ fact.name }}</label>
        </div>
      </div>
      <div class="image-settings-container">
        <div class="image-settings" >
          <div @click="moveImageSlider('left');saveDataToLocal();" class="image-settings-left">
          </div>
            <div class="image-settings-names" >
                <div id="imageSlider" ref="imageSlider" class="image-settings-name">
                  <p v-for="image in images" class="image-name">{{ image.name }}</p>
                </div>
            </div>
          <div @click="moveImageSlider('right');saveDataToLocal();" class="image-settings-right">
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
        },
        {
          name: "Forest",
          url: "/images/background02.png",
        },
        {
          name: "Puffy",
          url: "/images/background03.png",
        },
        {
          name: "Night Moon",
          url: "/images/background04.png",
        },
        {
          name: "Sunset Moon",
          url: "/images/background05.png",
        },
        {
          name: "Street",
          url: "/images/background06.png",
        },
        {
          name: "Coming Home",
          url: "/images/background07.png",
        },
        {
          name: "Traveling Home",
          url: "/images/background08.png",
        }
      ],
      isOptionsOpen: false,
      shortcuts: [
        {
          name: "general",
          links: [
            {
              name: "discord",
              url: "https://discord.com/channels/@me"
            },{
              name: "whatsapp",
              url: "https://discord.com/channels/@me"
            },
          ]
        },
        {
          name: "general",
          links: [
            {
              name: "discord",
              url: "https://discord.com/channels/@me"
            },
          ]
        },
        {
          name: "general",
          links: [
            {
              name: "discord",
              url: "https://discord.com/channels/@me"
            },
          ]
        },
        {
          name: "general",
          links: [
            {
              name: "discord",
              url: "https://discord.com/channels/@me"
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
    moveImageSlider(direction) {
      const currentIndex = this.images.indexOf(this.images.find( image => {
        return this.selectedImage.name == image.name;
      }));
      this.$refs.imageSlider.style.left = `-${ currentIndex * 200 }px`;
      // Moving the slider
      if (direction) {
        let nextIndex;
        if (direction == "left") {
          nextIndex = (currentIndex == 0) ? this.images.length - 1 : currentIndex - 1;
        } else if (direction == "right") {
          nextIndex = (currentIndex == this.images.length - 1) ? 0 : currentIndex + 1;
        }
        this.updateImage(this.images[nextIndex]);
        this.$refs.imageSlider.style.left = `-${ nextIndex * 200 }px`;
        this.saveDataToLocal();
      }
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
        this.moveImageSlider();
      }
      if (localStorage.selectedFactsType) {
        this.selectedFactsType = JSON.parse(localStorage.getItem("selectedFactsType"));
      }
      if (localStorage.shortcuts) {
        this.shortcuts = JSON.parse(localStorage.getItem("shortcuts"));
      }
    },
    saveDataToLocal() {
      localStorage.setItem("selectedImage", JSON.stringify(this.selectedImage));
      localStorage.setItem("selectedFactsType", JSON.stringify(this.selectedFactsType));
      localStorage.setItem("shortcuts", JSON.stringify(this.shortcuts));
    },
    modifyShortcuts(index, name, links) {
      this.shortcuts[index].name = name;
      this.shortcuts[index].links = links;
      this.saveDataToLocal()
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
  min-width: 200px;
  text-align: center;
}

.image-settings-name {
  display: flex;
  position: relative;
}

.image-settings-names {
  overflow: hidden;
  width: 200px;
  box-shadow: 0 0 0 2px var(--white);
  padding: 0.2rem 0rem;
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
