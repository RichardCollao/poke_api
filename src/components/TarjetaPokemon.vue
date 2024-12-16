<template>
    <div class="card text-center">
        <img :src="imagenPokemon" :alt="pokemon.nombre" class="card-img-top" :style="estiloImagen" />
        <div class="card-body">
            <h5 v-if="descubierto" class="card-title">{{ pokemon.nombre }}</h5>
            <div v-else>
                <input type="text" v-model="nombreUsuario" class="form-control mb-2" placeholder="Adivina el nombre" />
                <button class="btn btn-primary" @click="verificarNombre">Descubrir</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        pokemon: Object,
        alDescubrir: Function,
    },
    data() {
        return {
            nombreUsuario: '',
            descubierto: false,
        };
    },
    computed: {
        imagenPokemon() {
            return this.descubierto
                ? this.pokemon.imagen
                : this.pokemon.imagen + '?filter=blur(5px)';
        },
        estiloImagen() {
            return this.descubierto
                ? {}
                : { filter: 'blur(5px) grayscale(100%)' };
        },
    },
    methods: {
        verificarNombre() {
            if (this.nombreUsuario.toLowerCase() === this.pokemon.nombre.toLowerCase()) {
                this.descubierto = true;
                this.alDescubrir();
            } else {
                alert('Nombre incorrecto. Intenta de nuevo.');
            }
        },
    },
};
</script>

<style scoped>
.card {
    margin: 10px;
}
</style>