<template>
  <div class="pokemon-container">
    <div class="search-container">
      <q-input v-model="pokemonId" min="1" label="Número del Pokémon" outlined standout counter clearable
        @keydown.enter="fetchPokemon">
        <template v-slot:append>
          <q-icon name="favorite" />
        </template>
      </q-input>
      <q-btn @click="fetchPokemon" label="Buscar" color="primary" />
    </div>

    <q-card v-if="pokemon.name" class="pokemon-card">
      <q-card-section class="header">
        <div class="header-content">
          <h1>{{ capitalize(pokemon.name) }}</h1>
          <h2>#{{ pokemon.id }}</h2>
        </div>
        <img :src="imgPokemon" :alt="pokemon.name" />
      </q-card-section>

      <q-card-section class="pokemon-details">
        <div class="info">
          <p><strong>Altura:</strong> {{ pokemon.height / 10 }} m</p>
          <p><strong>Peso:</strong> {{ pokemon.weight / 10 }} kg</p>
          <p>
            <strong>Tipo:</strong>
            <span v-for="type in pokemon.types" :key="type.slot" :class="['type', 'type-' + type.type.name]">
              {{ capitalize(type.type.name) }}
            </span>
          </p>
        </div>

        <div class="stats">
          <h3>Estadísticas</h3>
          <ul>
            <li v-for="stat in pokemon.stats" :key="stat.stat.name" class="stat-item">
              <div class="stat-name">
                <strong>{{ capitalize(stat.stat.name) }}:</strong>
                <span>{{ stat.base_stat }} / 255</span>
              </div>
              <q-linear-progress :value="stat.base_stat / 255" class="tornasol-bar" color="grey"  track-color="grey-3"
                rounded size="10px" style="width: 100%; margin-top: 3px" />
            </li>
          </ul>
        </div>
      </q-card-section>
    </q-card>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";
import axios from "axios";
import { QInput, QBtn, QIcon, QCard, QCardSection, QLinearProgress } from "quasar";

const pokemon = ref({});
const imgPokemon = ref("");
const pokemonId = ref("");

async function fetchPokemon() {
  try {
    const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${pokemonId.value}`);
    pokemon.value = response.data;
    imgPokemon.value = response.data.sprites.other["official-artwork"].front_default;
  } catch (error) {
    console.error("Error fetching Pokemon data:", error);
  }
}

function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

const primaryTypeClass = ref("");

watch(
  () => pokemon.value,
  (newVal) => {
    if (newVal.types && newVal.types.length > 0) {
      primaryTypeClass.value = newVal.types[0].type.name;
      document.body.className = primaryTypeClass.value;
    } else {
      document.body.className = "";
    }
  }
);


</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Seymour+One&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

.pokemon-container {
  max-width: 900px;
  /* Increased max-width for larger screen sizes */
  margin: auto;
  padding: 20px;
  font-family: 'Roboto', sans-serif;
}

.search-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.q-input {
  flex: 1;
  margin-right: 10px;
}

.pokemon-card {
  display: flex;
  flex-direction: row;
  text-align: center;
  margin: auto;
  padding: 20px;
  /* Increased padding */
  width: 85vw;
  /* Increased width */
  max-width: 900px;
  /* Increased max-width */
  height: 600px;
  /* Added height to make the card a bit longer */
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.header {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.header-content {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.header h1 {
  margin: 0;
  font-family: 'Seymour One', sans-serif;
  font-size: 56px;
}

.header h2 {
  margin: 0;
  font-family: 'Seymour One', sans-serif;
  font-weight: 700;
  color: rgb(255, 38, 38);
  margin-top: 50px;
}

.header img {
  width: 250px;
  /* Increased image size */
  height: auto;
  margin-top: 50px;
}

.pokemon-details {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  margin-top: 10px;
}

.info p {
  margin: 15px 0;
}

.type {
  display: inline-block;
  padding: 4px 8px;
  margin-right: 3px;
  border-radius: 3px;
  font-weight: bold;
  color: white;
}

.stats {
  width: 100%;
  margin-top: 50px;
}

.stats h3 {
  margin: 0;
  font-family: 'Seymour One', sans-serif;
  font-size: 35px;
  font-weight: 700;
}

.stat-item {
  margin-bottom: 8px;
  list-style: none;
}

.stat-name {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

body {
  font-size: 16px;
}

body.normal {
  background-color: #A8A77A;
}

body.fire {
  background-color: #EE8130;
}

body.water {
  background-color: #6390F0;
}

body.electric {
  background-color: #F7D02C;
}

body.grass {
  background-color: #7AC74C;
}

body.ice {
  background-color: #96D9D6;
}

body.fighting {
  background-color: #C22E28;
}

body.poison {
  background-color: #A33EA1;
}

body.ground {
  background-color: #E2BF65;
}

body.flying {
  background-color: #A98FF3;
}

body.psychic {
  background-color: #F95587;
}

body.bug {
  background-color: #A6B91A;
}

body.rock {
  background-color: #B6A136;
}

body.ghost {
  background-color: #735797;
}

body.dragon {
  background-color: #6F35FC;
}

body.dark {
  background-color: #705746;
}

body.steel {
  background-color: #B7B7CE;
}

body.fairy {
  background-color: #D685AD;
}

.type-normal {
  background-color: #A8A77A;
}

.type-fire {
  background-color: #EE8130;
}

.type-water {
  background-color: #6390F0;
}

.type-electric {
  background-color: #F7D02C;
}

.type-grass {
  background-color: #7AC74C;
}

.type-ice {
  background-color: #96D9D6;
}

.type-fighting {
  background-color: #C22E28;
}

.type-poison {
  background-color: #A33EA1;
}

.type-ground {
  background-color: #E2BF65;
}

.type-flying {
  background-color: #A98FF3;
}

.type-psychic {
  background-color: #F95587;
}

.type-bug {
  background-color: #A6B91A;
}

.type-rock {
  background-color: #B6A136;
}

.type-ghost {
  background-color: #735797;
}

.type-dragon {
  background-color: #6F35FC;
}

.type-dark {
  background-color: #705746;
}

.type-steel {
  background-color: #B7B7CE;
}

.type-fairy {
  background-color: #D685AD;
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .pokemon-card {
    width: 90vw;
    /* Adjusted width for smaller screens */
    padding: 20px;
    flex-direction: column;
    /* Stacked layout on smaller screens */
    height: auto;
    /* Adjusted height */
  }

  .header img {
    width: 150px;
    /* Adjusted image size for smaller screens */
  }

  .stat-name {
    flex-direction: column;
    align-items: flex-start;
  }

  .stats {
    margin-top: 20px;
  }

  .header-content {
    margin-bottom: 20px;
  }
}

@media (max-width: 480px) {
  .header h1 {
    font-size: 36px;
  }

  .header h2 {
    font-size: 38px;
  }

  .info p {
    font-size: 14px;
  }

  .stats h3 {
    font-size: 20px;
  }

  .header img {
    width: 120px;
  }
}
</style>