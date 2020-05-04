<template>
  <div class="detail-page card">
    <div class="detail-view container" v-if="show">
      <div v-if="pokemon" >
        <img :src="imageUrl + pokemon.id + '.png'" />
      </div>
      <div v-if="pokemon" class="data card-body">
        <h4 class="card-header text-uppercase">{{ pokemon.name }}</h4>
        <div class="property row">
          <span class="col">Base Experience: {{ pokemon.base_experience }} XP</span>
        </div>
        <h5 class="card-header text-uppercase">Pokemon Types</h5>
        <div class="types">
          <div class="type"
            v-for="(value, index) in pokemon.types"
            :key="'value' + index"
          >
            {{ value.type.name }}
          </div>
        </div>
        <h5 class="card-header text-uppercase">Abilities</h5>
        <div class="abilities row">
          <div
            class="ability col"
            v-for="(value, index) in pokemon.abilities"
            :key="'value' + index"
          >
            {{ value.ability.name }}
          </div>
        </div>
        <h5 class="card-header text-uppercase">Moves</h5>
        <div class="moves row">
          <div class="move col-md-2"
            v-for="(value, index) in pokemon.moves"
            :key="'value' + index"
          >
            {{ value.move.name }}
          </div>
        </div>
        <h5 class="card-header text-uppercase">Order Number</h5>
        <div class="order" v-if="pokemon">
          <p>{{ pokemon.order }}</p>
        </div>

        <h5 class="card-header text-uppercase">Stats</h5>
        <div class="Stats row">
          <div class="stat col"
            v-for="(value, index) in pokemon.stats"
            :key="'value' + index"
          >
            {{ value.stat.name }}
          </div>
        </div>
      </div>
      <span v-else>The pokemon was not found</span>
      <button class="btn btn-danger" @click="closeDetail">close</button>
    </div>
    <i v-else class="fas fa-spinner fa-spin"></i>
    </div>
</template>

<script>
export default {
  props: ["pokemonUrl", "imageUrl"],
  data: () => {
    return {
      show: false,
      pokemon: {},
    };
  },
  methods: {
    fetchData() {
      const req = new Request(this.pokemonUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) return resp.json();
        })
        .then((data) => {
          this.pokemon = data;
          this.show = true;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    closeDetail() {
      this.$emit("closeDetail");
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<style lang="scss" scoped>
.card-header{
    padding: 0;
  }
  .detail-page {
    position: fixed;
    font-size: 14px;
    top: 0;
    height: 100vh;
    width: 100vw;
    background-color: #f4f4f4;
    color: #000;
    .card-header {
      color: #000;
      background-color: #999;
    }
  }
</style>
