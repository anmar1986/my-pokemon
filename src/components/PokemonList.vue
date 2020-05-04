<template>
  <div class="container">
    <div class="row">
      <article
        class="col-lg-3 col-md-6"
        v-for="(pokemon, index) in pokemons"
        :key="'poke' + index"
        @click="setPokemonUrl(pokemon.url)"
      >
        <div class="card content border-info">
          <div>
            <img class="img" :src="imageUrl + pokemon.id + '.png'" alt="" />
          </div>
          <div class="card-body">
            <h3 class="card-title">{{ pokemon.name }}</h3>
          </div>
        </div>
      </article>
      <div id="scroll-trigger" ref="infinitescrolltrigger">
        <i class="fas fa-spinner fa-spin"></i>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["imageUrl", "apiUrl"],
  data: () => {
    return {
      pokemons: [],
      nextUrl: "",
      currentUrl: "",
    };
  },
  methods: {
    fetchData() {
      const req = new Request(this.currentUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) return resp.json();
        })
        .then((data) => {
          this.nextUrl = data.next;
          data.results.forEach((pokemon) => {
            pokemon.id = pokemon.url
              .split("/")
              .filter(function(part) {
                return !!part;
              })
              .pop();
            this.pokemons.push(pokemon);
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    scrollTrigger() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.intersectionRatio > 0 && this.nextUrl) {
            this.next();
          }
        });
      });

      observer.observe(this.$refs.infinitescrolltrigger);
    },
    next() {
      this.currentUrl = this.nextUrl;
      this.fetchData();
    },
    setPokemonUrl(url) {
      this.$emit("setPokemonUrl", url);
    },
  },
  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  },
  mounted() {
    this.scrollTrigger();
  },
};
</script>

<style lang="scss" scoped>
article {
  cursor: pointer;
}
.card {
  margin-bottom: 30px;
}
</style>
