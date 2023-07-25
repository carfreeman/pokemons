<script setup>
    import { useRoute, useRouter } from 'vue-router';
    import { useGetData } from '../composables/getData';
    import { useFavoriteStore } from '../store/favorites';

    const route = useRoute();
    const router = useRouter();
    const useFavorite = useFavoriteStore();

    const back = () => {
        router.push('/pokemons');
    }

    const { add, findPoke } = useFavorite;

    const { data, error, loading, getData } = useGetData();
    
    getData(`https://pokeapi.co/api/v2/pokemon/${route.params.name}`);
</script>

<template>
    <div v-if="loading">Cargando...</div>
    <div v-else>
        <div v-if="data">
            <img :src="data.sprites?.front_default" />
            <h1>{{ $route.params.name }}</h1>
            <button
                class="btn btn-outline-warning mb-2"
                @click="add(data)"
                :disabled="findPoke(data.name)">
                Agregar Favorito
            </button>
        </div>
        <h1 v-if="error">No existe el pokemon</h1>
        <button @click="back" class="btn btn-outline-primary">Volver</button>
    </div>
</template>