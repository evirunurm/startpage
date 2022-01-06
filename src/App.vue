<template>
<div class="background">
  <div class="container">
    <section class="top">
      <article class="left">
        <div class="today-info">
          <h1 class="date font--light">{{ this.date }}</h1>
          <div class="clock">
            <h2>{{ this.time }}</h2>
          </div>
        </div>
        <p class="fact font--light"> {{selectedFactsType.fact}} </p>
        <button v-if="selectedFactsType.id != 'off'" @click="updateFact(selectedFactsType);" type="button" name="button">Next Fact</button>
      </article>
      <article class="right">
        <div class="img-container">
          <img class="img" :src="selectedImage.url" alt="">
        </div>
      </article>
    </section>
    <div class="separator"></div>
    <section class="bottom">
      <links-container v-on:deleteContainer="deleteShortcut" v-on:modifyShortcuts="modifyShortcuts" v-for="shortcut in shortcuts" :name=shortcut.name :links=shortcut.links :index="shortcuts.indexOf(shortcut)"/>
      <div v-if="shortcuts.length < 4" class="add-link-container" @click="addShortcut">
        <svg width="18" height="18" viewBox="0 0 43 43" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M22 4V39" stroke="white" stroke-width="8" stroke-linecap="round" stroke-linejoin="round"/>
          <path d="M39 22L4 22" stroke="white" stroke-width="8" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </div>
    </section>
  </div>
  <div id="settings" class="settings-container" >
    <div class="settings-button"  @click="isOptionsOpen = !isOptionsOpen">
      <svg width="35" height="35" viewBox="0 0 64 68" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path fill-rule="evenodd" clip-rule="evenodd" d="M22.6147 4.05687C23.067 1.70215 25.1272 0 27.5249 0H36.627C39.0834 0 41.176 1.78426 41.5642 4.20975L42.539 10.2998C44.189 11.0583 45.7446 11.987 47.1833 13.0634L52.8445 11.124C55.1061 10.3492 57.5997 11.2837 58.7951 13.3541L63.1526 20.9016C64.3778 23.0237 63.8846 25.7209 61.9878 27.2721L57.3477 31.0669C57.4289 31.8971 57.4704 32.7389 57.4704 33.5904C57.4704 34.7404 57.3947 35.8726 57.2479 36.9824L61.9461 40.7708C63.8638 42.3171 64.3694 45.0297 63.1377 47.1631L58.8123 54.6549C57.6093 56.7386 55.0927 57.67 52.823 56.8716L46.4658 54.6353C45.6284 55.218 44.7545 55.752 43.8483 56.2331L42.627 63.0612C42.2006 65.4452 40.127 67.1809 37.7051 67.1809H27.6012C25.2442 67.1809 23.2072 65.5347 22.7126 63.2301L21.3701 56.9755C19.93 56.3291 18.5604 55.5535 17.2761 54.6636L10.9993 56.8716C8.72959 57.67 6.21305 56.7386 5.01001 54.6549L0.68459 47.1631C-0.547114 45.0297 -0.0414361 42.3171 1.87622 40.7708L6.46515 37.0706C6.3106 35.9326 6.23077 34.7708 6.23077 33.5904C6.23077 32.7066 6.27552 31.8333 6.36288 30.9726L1.83584 27.272C-0.0616392 25.721 -0.55522 23.0232 0.670158 20.9008L5.0267 13.355C6.22227 11.2843 8.71667 10.3498 10.9785 11.1254L16.5533 13.0368C18.0626 11.9117 19.7 10.9489 21.4397 10.1743L22.6147 4.05687ZM31.8301 18.2185C23.3499 18.2296 16.4787 25.1076 16.4787 33.5904C16.4787 42.0801 23.361 48.9623 31.8506 48.9623C40.3403 48.9623 47.2225 42.0801 47.2225 33.5904C47.2225 25.1076 40.3514 18.2296 31.8711 18.2185H31.8301Z" fill="white"/>
      </svg>
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
          <div class="image-settings-arrow" @click="moveImageSlider('left');saveDataToLocal();">
            <svg width="20" height="20" viewBox="0 0 35 49" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M3.52393 18.577L21.6225 2.14536C26.7627 -2.5214 35 1.12576 35 8.06841V40.9316C35 47.8742 26.7627 51.5214 21.6225 46.8546L3.52393 30.423C0.0272703 27.2484 0.0272703 21.7516 3.52393 18.577Z" fill="white"/>
            </svg>
          </div>
            <div class="image-settings-names" >
                <div id="imageSlider" ref="imageSlider" class="image-settings-name">
                  <p v-for="image in images" class="image-name">{{ image.name }}</p>
                </div>
            </div>
          <div class="image-settings-arrow" @click="moveImageSlider('right');saveDataToLocal();">
            <svg width="20" height="20" viewBox="0 0 35 49" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M31.4761 18.577L13.3775 2.14536C8.2373 -2.5214 0 1.12576 0 8.06841V40.9316C0 47.8742 8.2373 51.5214 13.3775 46.8546L31.4761 30.423C34.9727 27.2484 34.9727 21.7516 31.4761 18.577Z" fill="white"/>
            </svg>
          </div>
        </div>
      </div>
      <div class="color-settings">
        <p>Colors :</p>
        <div>
          <label for="--primaryColor">Primary</label>
          <input @input="updateColors()" type="color" id="--primaryColor" v-model="colors.primary">
        </div>
        <div>
          <label for="--secondaryColor">Secondary</label>
          <input @input="updateColors()" type="color" id="--secondaryColor" v-model="colors.secondary">
        </div>
        <div>
          <label for="--backgroundColor">Background</label>
          <input @input="updateColors()" type="color" id="--backgroundColor" v-model="colors.background">
        </div>
        <div>
          <label for="--fontColorColor">Font</label>
          <input @input="updateColors()" type="color" id="--fontColorColor" v-model="colors.font">
        </div>
      </div>
      <p class="credits">Developed by <a target="_blank" href="https://github.com/evirunurm">Evelin Virunurm</a></p>
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
              name: "whatsapp web",
              url: "https://web.whatsapp.com/"
            },
          ]
        },
        {
          name: "social",
          links: [
            {
              name: "instagram",
              url: "https://www.instagram.com/"
            },
            {
              name: "reddit",
              url: "https://www.reddit.com/"
            },
          ]
        },
        {
          name: "video",
          links: [
            {
              name: "youtube",
              url: "https://www.youtube.com/"
            },
            {
              name: "netflix",
              url: "https://www.netflix.com/browse"
            },
          ]
        }
      ],
      colors: {
        primary: "#ff90cc",
        secondary: "#57455d",
        background: "#2c2c2c",
        font: "#b9b9b9",
      },
      time: "",
      date: "",
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
        let res = await fetch(url, {
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
      if (localStorage.colors) {
        this.colors = JSON.parse(localStorage.getItem("colors"));
      }
    },
    saveDataToLocal() {
      localStorage.setItem("selectedImage", JSON.stringify(this.selectedImage));
      localStorage.setItem("selectedFactsType", JSON.stringify(this.selectedFactsType));
      localStorage.setItem("shortcuts", JSON.stringify(this.shortcuts));
      localStorage.setItem("colors", JSON.stringify(this.colors));
    },
    modifyShortcuts(index, name, links) {
      this.shortcuts[index].name = name;
      this.shortcuts[index].links = links;
      this.saveDataToLocal()
    },
    prepareToClosePopup() {
      document.body.addEventListener("mousedown", (event) => {
        this.closePopup(event);
      })
    },
    closePopup(event) {
      let popup = document.getElementById(`settings`);
      if (event.target === popup || popup.contains(event.target)) {
        return;
      }
      this.isOptionsOpen = false;
    },
    updateColors() {
      for (const color in this.colors) {
        document.querySelector(':root').style.setProperty(`--${color}Color`, this.colors[color]);
      }
      this.saveDataToLocal();
    },
    getDate() {
      const months = [
        'January',
        'February',
        'March',
        'April',
        'May',
        'June',
        'July',
        'August',
        'September',
        'October',
        'November',
        'December'
      ];
      const date = new Date();
      let day = date.getDate();
      let dateString = "";

      switch (date.getDay()) {
        case 0:
          dateString += "Sunday, ";
          break;
        case 1:
          dateString += "Monday, ";
          break;
        case 2:
          dateString += "Tuesday, ";
          break;
        case 3:
          dateString += "Wednesday, ";
          break;
        case 4:
          dateString += "Thursday, ";
          break;
        case 5:
          dateString += "Friday, ";
          break;
        case 6:
          dateString += "Saturday, ";
          break;
      }
      dateString += day;
      if (day === 1) {
        dateString +=  "st"
      }else if (day === 2) {
        dateString +=  "nd"
      } else if (day === 3) {
        dateString +=  "rd"
      } else {
        dateString +=  "th"
      }
      dateString += " " + months[date.getMonth()].slice(0,3) + " " + date.getFullYear();
      this.date = dateString;
    },
    getTime() {
      const date = new Date();
      this.time = date.getHours().toString().padStart(2,'0') + ":" + date.getMinutes().toString().padStart(2,'0');
    },
    deleteShortcut(index) {
      console.log("done")
      this.shortcuts.splice(index,1);
      this.saveDataToLocal();
    },
    addShortcut() {
      if (this.shortcuts.length < 4) {
        this.shortcuts.push({
          name: "new",
          links: []
        });
      } else {
        alert("There's a maximum amount of 4 folders, sorry:))");
      }
      this.saveDataToLocal();
    }
  },
  mounted() {
    this.getLocalData();
    this.prepareToClosePopup();
    this.updateColors();
    this.getDate();
    this.getTime();
    setInterval( () => {
      this.getTime()
    }, 1000);
  },
  components: {
    LinksContainer,
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+Display:wght@100;200;300;400;500;600&display=swap');

rect, path, svg {
  stroke: var(--fontColor);
  fill: var(--fontColor);
}

.save svg, .save path {
  fill: none;
}

:root {
  --black: rgba(0,0,0)
}

::selection {
  color: var(--black);
  background: var(--primaryColor);
}

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: 'Noto Sans Display', sans-serif;
}

body {
  background-color: var(--backgroundColor);
  color: var(--fontColor);
  display: flex;
}

a {
  color: var(--fontColor);
  text-decoration: none;
}

a:hover {
  text-decoration: line-through;
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
  max-width: 700px;
  gap: 3rem;
}

.top {
  display: flex;
  gap: 2.5rem;
  width: 100%;
  justify-content: center;
}

.left {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  text-align: right;
  gap: 1rem;
}

.left button {
  font-size: 1rem;
  border: none;
  align-self: end;
  padding: 0.5rem 1.75rem;
  background: var(--secondaryColor);
  color: var(--primaryColor);
  cursor: pointer;
  transition: 0.08s ease-in-out;
}

.left button:hover {
  background: var(--primaryColor);
  color: var(--secondaryColor)
}

.separator {
  height: 1px;
  background: var(--fontColor);
  width: 100%;
  opacity: 40%;
}

.right {
  display: flex;
  align-items: flex-start;
}

.img-container {
  height: 300px;
  width: 300px;
  overflow: hidden;
  border: 1px solid var(--fontColor);
}

.img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.bottom {
  display: flex;
  justify-content: space-between;
  width: 100%;
  text-align: center;
  position: relative;
}

.clock h2 {
  font-size: 3em;
  font-weight: 500;
  opacity: 50%;
}

.image-settings {
  display: flex;
  align-items: center;
  gap: 10px;
}

.image-settings-name p {
  min-width: 200px;
  text-align: center;
}

.image-settings-name {
  display: flex;
  position: relative;
  user-select: none;
}

.image-settings-names {
  overflow: hidden;
  width: 200px;
  box-shadow: 0 0 0 2px var(--fontColor);
  padding: 0.2rem 0rem;
}

.settings-button {
  cursor: pointer;
  opacity: 20%;
  transition: opacity 0.07s ease-in-out;
}

.settings-button:hover {
  opacity: 80%;
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
  background: var(--backgroundColor);
  border: 1px solid var(--fontColor);
  padding: 1rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.image-settings-arrow {
  cursor: pointer;
  opacity: 20%;
  transition: opacity 0.08s ease-in-out;
}

.image-settings-arrow:hover {
  opacity: 80%;
}

.facts-settings {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.facts-settings__fact {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
.facts-settings__fact input, .facts-settings__fact label {
  cursor: pointer;
}
.facts-settings p {
  margin-bottom: 0.25rem;
}

.color-settings {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.color-settings p {
  margin-bottom: 0.5rem;
}

.color-settings div {
  display: flex;
  gap: 1rem;
  justify-content: space-between;
}

.color-settings div input, .color-settings div label {
  border: none;
  cursor: pointer;
}

.add-link-container {
  cursor: pointer;
  position: absolute;
  right: 0;
  opacity: 10%;
  transition: opacity 0.08s ease-in-out;
}

.add-link-container:hover {
  opacity: 80%;
}
</style>
