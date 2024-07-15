<script setup lang="ts">
    import{ref} from "vue";
    const id = ref(0);
    const props = defineProps<{
        pokemon: any,
    }>();
    
    id.value = Date.now();

</script>

<template>
    <div class="pokemon-info">
        <h2>{{ props.pokemon.name }}</h2>
        <div class="pokemon-info-infobox">
            <div class="pokemon-info-infobox-images">
                <img :src="props.pokemon?.sprites?.front_default"/>
                <img :src="props.pokemon?.sprites?.back_default"/>
            </div>
            <div class="pokemon-info-infobox-images">
                <img :src="props.pokemon?.sprites?.other?.showdown?.front_default"/>
                <img :src="props.pokemon?.sprites?.other?.showdown?.back_default"/>
            </div>
            <p>Type: {{ props.pokemon?.types[0]?.type?.name }} |  Date: {{ id }}</p>
            <a :href="props.pokemon?.url">{{ props.pokemon?.url }}</a>
            <ul>
                <li v-for="encounter in props.pokemon.encounters" >
                    {{ encounter.location_area?.name }}
                </li>
            </ul>
        </div>
    </div>
</template>


<style scooped>
.pokemon-info {
    width: 100vw;
    display: flex;
    flex-direction: column;
    align-items: center;
}
.pokemon-info:hover {
    border: 1px solid hsla(160, 100%, 37%, 1);
    width: 100vw;
}
.pokemon-info-infobox {
    display: flex;
    flex-direction: column;
}
.pokemon-info-infobox-images {
    display: flex;
    flex-direction: row;
}
</style>
