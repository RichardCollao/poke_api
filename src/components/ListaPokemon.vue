<template>
    <div class="container">
        <h1 class="text-center my-4">¿Quién es este Pokémon?</h1>
        <hr>
        <div class="row">
            <div v-for="pokemon in pokemones" :key="pokemon.nombre" class="col-md-3">
                <TarjetaPokemon :pokemon="pokemon" :alDescubrir="incrementarContador" />
            </div>
        </div>
        <h3 class="text-center mt-4">
            Pokémons descubiertos: {{ contadorDescubiertos }}/20
        </h3>
    </div>
</template>

<script>
import axios from 'axios';
import TarjetaPokemon from './TarjetaPokemon.vue';

export default {
    components: { TarjetaPokemon },
    data() {
        return {
            pokemones: [],
            contadorDescubiertos: 0,
        };
    },
    methods: {
        async obtenerPokemones() {
            try {
                const respuesta = await axios.get('https://pokeapi.co/api/v2/pokemon');
                this.pokemones = await Promise.all(
                    respuesta.data.results.map(async (pokemon) => {
                        const detalles = await axios.get(pokemon.url);
                        return {
                            nombre: pokemon.name,
                            imagen: detalles.data.sprites.front_default,
                        };
                    })
                );
            } catch (error) {
                console.error('Error al obtener los datos de los Pokémon:', error);
            }
        },
        incrementarContador() {
            this.contadorDescubiertos++;
        },
    },
    created() {
        this.obtenerPokemones();
    },
};
</script>