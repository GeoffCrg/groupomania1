<template>
  <div id="app">
    <header>
      <Header />
      <div class="md-primarys">
        <router-link to="/allpost"
          ><span class="color">Accueil</span></router-link
        >

        <router-link to="/profil"
          ><span class="color">Profils</span></router-link
        >
        <router-link to="/myprofil"
          ><span class="color">Mon profil</span></router-link
        >

        <a href="#" v-on:click="exitUser"><span class="color">Deconnexion</span></a>
      </div>
    </header>

    <body class="bg-body">
      <div class="connect" v-if="id === null">
        <router-link to="/signup">Inscription</router-link> ||
        <router-link to="/login">Connexion</router-link>
      </div>
      <router-view />
    </body>

    <footer>
      <Footer />
    </footer>
  </div>
</template>

<script>
import Header from "./components/Header";
import Footer from "./components/Footer";
export default {
  name: "App",
  components: {
    Header,
    Footer,
  },
  data() {
    return {
      id: "",
    };
  },
  mounted() {
    let idUser = localStorage.getItem("Id");
    
    console.log(idUser);
    this.id = idUser;
  },
  methods: {
    exitUser() {
      localStorage.removeItem("Id");
      localStorage.removeItem("token");
      localStorage.removeItem("email");
      location.replace(location.origin + "/signup#/signup");
      location.reload();
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
.md-primarys{
margin-top: 10px;
}
.connect {
  margin-bottom: 100px;
  margin-top: 200px;
}
.md-primarys {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
}
.color {
  color: #1e81b0;
  font-size: 20px;
  
}
.connect{
  font-size: 20px;
}
@media all and (max-width: 599px) {
  .bg-body {
    margin-bottom: 100px;
  }
  .md-primary {
    display: flex;
    flex-direction: column;
  }
  .connect {
    margin-top: 100px;
  }
}
</style>
