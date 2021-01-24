
<template>

  <v-app>
  <nav class="nav-extended">
    <div class="nav-wrapper light-blue darken-4">      
      <ul class="left hide-on-med-and-down light-blue darken-4">
      </ul>
      <ul class="right hide-on-med-and-down light-blue darken-4">
        <li> <a> <router-link to="/">Home</router-link> </a> </li>
        <li> <a> <router-link to="/tutorial">Tutorial2</router-link> </a>  </li>
        <li> <a> <router-link to="/about">About</router-link> </a>  </li>
      </ul>
    </div>
  </nav>
  <a class="btn-floating btn-large halfway-fab waves-effect waves-light teal">
    <i class="material-icons">add</i>
  </a>
</template>



<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Ceerma Logo"
          class="shrink mr-2"
          contain
          src="./assets/logo_ceerma.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Ceerma Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="./assets/logo_ceerma_text.png"
          width="100"
        />
      
      </div>
      <div style="font-size:17px">
        <v-btn plain icon v-on:click="changelanguage('br')" > <flag iso="br" /> </v-btn>	
        <v-btn plain icon v-on:click.prevent="changelanguage('en')"> <flag iso="us" /> </v-btn>
        <v-btn plain icon v-on:click.prevent="changelanguage('es')"> <flag iso="cl" /> </v-btn>
      </div>    
      
      <v-spacer></v-spacer>

      <v-toolbar-items>  
        <v-btn target="_blank"  text>
          <router-link :to="`/${$i18n.locale}`" class="white--text"> {{ $t('nav.home') }}  </router-link>
        </v-btn>
        <v-btn target="_blank"  text>
          <router-link :to="`/${$i18n.locale}/about`" class="white--text">About</router-link>
        </v-btn>
      </v-toolbar-items>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template> 

<script>



export default {
  name: 'App',


  data: () => ({
    //
  }),


  methods: {
    changelanguage(lang) {
      if (this.$i18n.locale != lang) {
        this.$i18n.locale = lang
        this.$router.push({
          params: { lang: lang }})
      }
    }

  }
};
</script>
