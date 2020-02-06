<template>
  <div class="home">
    <div class="options">
      <form>
        <div class="site-input">
          <input type="radio" id="site-a" value="reddit" v-model="site" />
          <label for="site-a">Reddit</label>

          <input type="radio" id="site-b" value="twitter" v-model="site" />
          <label for="site-b">Twitter</label>

          <input type="radio" id="site-c" value="gab" v-model="site" />
          <label for="site-c">GAB</label>
        </div>

        <div class="term-input">
          <input type="text" id="term" v-model="term" />
          <label for="term">search term</label>
        </div>
      </form>
    </div>
    <div class="results"></div>
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <!-- <HelloWorld msg="beep boop" /> -->
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import { debounce } from "lodash";

export default {
  name: "home",
  components: {
    // HelloWorld
  },
  data() {
    return {
      site: "reddit",
      term: "",
      limit: 20
    };
  },
  computed: {
    query() {
      if (this.term.length < 3) return null;

      return `?site=${this.site}&term=${this.term}&limit=${this.limit}`;
      // this is string interpolation
      // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#Expression_interpolation
    }
  },
  watch: {
    query(newValue) {
      console.log("[query]", newValue);
      if (!newValue) return;

      const url = `https://smat-be.herokuapp.com/${newValue}`;
      this.fetchData(url);
    }
  },
  methods: {
    /* a simple version of fetchData */
    // fetchData: function(url) {
    //   console.log(url)
    //   fetch(url)
    //     .then(response => response.json())
    //     .then(results => {
    //       this.results = results
    //     })
    //     .catch(err => console.error(err));
    // }

    /* chill version which only triggers 300ms of quiet */
    // read about debounce: https://css-tricks.com/debouncing-throttling-explained-examples/#article-header-id-0
    fetchData: debounce(function(url) {
      console.log("[fetchData]", url);

      fetch(url)
        .then(response => response.json())
        .then(results => {
          // set the state
          this.results = results;
        })
        .catch(err => console.error(err));
    }, 300)
  }
};
</script>

<style scoped lang="scss">
form {
  .site-input {
    label {
      margin-right: 20px;
    }
  }
}
</style>
