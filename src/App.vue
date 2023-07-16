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
          <button v-if="selectedFactsType.id != 'off'" @click="$refs.settings.updateFact(selectedFactsType);" type="button" name="button">Next Fact</button>
        </article>
        <article class="right">
          <div class="img-container">
            <img class="img" :src="selectedImage.url" alt="">
          </div>
        </article>
      </section>
      <div class="separator"></div>
      <section class="bottom">
        <links-container v-on:deleteContainer="deleteShortcut" v-on:modifyShortcuts="modifyShortcuts" v-for="shortcut in shortcuts" :name=shortcut.name :links=shortcut.links :index="shortcuts.indexOf(shortcut)" v-bind:key="shortcut.name"/>
        <div title="Add a section" v-if="shortcuts.length < 4" class="add-link-container" @click="addShortcut">
          <svg width="18" height="18" viewBox="0 0 43 43" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M22 4V39" stroke="white" stroke-width="8" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M39 22L4 22" stroke="white" stroke-width="8" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </div>
      </section>
    </div>
  </div>
  <settings :colors="colors" 
    :facts-types="factsTypes" 
    :selected-facts-type="selectedFactsType" 
    :images="images" 
    :is-options-open="isOptionsOpen" 
    :selected-image="selectedImage"
    @save-data="saveDataToLocal"
    @update-image="updateImage"
    ref="settings"></settings>
</template>

<script >
  import LinksContainer from "./components/links_container.vue";
  import Settings from "./components/settings.vue";

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
        customImage: null
      }
    },
    methods: {
      updateImage(image) {
        this.selectedImage.url = image.url;
        this.selectedImage.name = image.name;
      },
      getLocalData() {
       if (localStorage.selectedImage) {
          this.selectedImage = JSON.parse(localStorage.getItem("selectedImage"));
          this.$refs.settings.moveImageSlider();
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
        if (localStorage.customImage) {
          this.customImage = JSON.parse(localStorage.getItem("customImage"));
        }
      },
      saveDataToLocal() {
        try {
          localStorage.setItem("selectedImage", JSON.stringify(this.selectedImage));
        } catch {
          alert("The provided image is too heavy to save it:( Sorry!");
        }
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
      },
      
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
      Settings
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

  button {
    font-size: 1rem;
    border: none;
    align-self: end;
    padding: 0.4rem 1.25rem;
    background: var(--secondaryColor);
    color: var(--primaryColor);
    cursor: pointer;
    transition: 0.08s ease-in-out;
  }

  button:hover {
    background: var(--primaryColor);
    color: var(--secondaryColor)
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
    top: -1rem;
    opacity: 25%;
    transition: opacity 0.1s ease-in-out;
  }

  .add-link-container:hover {
    opacity: 100%;
  }

  @media (max-width: 700px) {
    .top {
      flex-direction: column-reverse;
      gap: 1em;

    }

    .right {
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 3em 0 0 0 ;
    }

    .img-container {

    }

  }
</style>
