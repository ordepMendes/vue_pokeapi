<script setup>
    let pokemon = defineProps(["name", "urlImage"]);
</script>

<template>
    <div class="col-4">
        <div class="card p-2 mb-4">
            <img :src="urlImage" class="card-img-top" height="80" alt="...">
            <p class="text-center">{{pokemon.name}}</p>
        </div>
    </div>
</template>
