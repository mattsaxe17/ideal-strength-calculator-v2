<template>
  <v-app>
    <Nuxt />

    <footer>
      <div id="footer-content">
        <div>
          <p><b>Site designed and built by Matthew Saxe</b></p>
          <div>
            <a href="https://matthewsaxe.com"><v-icon large>mdi-web</v-icon></a>
            <a href="mailto:mattsaxe17@gmail.com"><v-icon large>mdi-email</v-icon></a>
            <a href="https://www.facebook.com/mattsaxe17"><v-icon large>mdi-facebook</v-icon></a>
            <a href="https://www.linkedin.com/in/mattsaxe17"><v-icon large>mdi-linkedin</v-icon></a>
            <a href="https://twitter.com/mattsaxe17"><v-icon large>mdi-twitter</v-icon></a>
          </div>
        </div>
        <div>
          <p id="disclaimer">
            *Calculations are meant to provide a baseline of weight and repetition values based on common research on
            the ratios of ideal strength between certain exercises. The results may not accurately portray your specific
            case, and do NOT warrant the attempt of any given exercise using a certain weight. Please use these values
            as a baseline, conduct more research, and weight train safely and responsibly.
          </p>
        </div>
      </div>
    </footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {};
  },
};
</script>

<style lang="scss">
#app {
  background: linear-gradient(105deg, var(--v-primary-darken3) 0%, var(--v-primary-darken2) 100%) !important;
  background-repeat: no-repeat;
  background-attachment: cover;
  background-attachment: fixed;
}

#disclaimer {
  font-size: 0.75em;
}

footer {
  border-top: 3px solid var(--v-accent-lighten3);
  text-align: center;
  display: flex;
  justify-content: center;
  padding: 2em;
  width: 100%;
  background-color: var(--v-background-base);

  #footer-content {
    display: flex;
    flex-direction: column;
    max-width: 700px;

    & > div {
      padding-bottom: 2em;
    }
  }

  a {
    text-decoration: none;

    i:before {
      color: var(--v-accent-base);
    }

    i:hover:before {
      color: var(--v-primary-base);
      font-size: 1.25em;
    }
  }
}
</style>
