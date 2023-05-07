<template>
    <div class="card" @click="handleClick" :class="{ flipped: isFlipped }">
        <div class="card-face card-face--front"></div>
        <div class="card-face card-face--back">
            <img :src="imageSrc" :alt="imageAlt" />
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
    props: {
        imageSrc: String,
        imageAlt: String,
        isFlipped: Boolean,
    },
    methods: {
        handleClick() {
            this.$emit("card-clicked");
        },
    },
});
</script>

<style scoped>
.card {
    width: 100%;
    height: 0;
    padding-bottom: 100%;
    position: relative;
    transform-style: preserve-3d;
    transition: transform 0.5s;
    cursor: pointer;
    user-select: none;
}

.card.flipped {
    transform: rotateY(180deg);
}

.card-face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    background-color: #f8f8f8;
    border-radius: 4px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.card-face--front {
    transform: rotateY(0deg);
}

.card-face--back {
    transform: rotateY(180deg);
}

img {
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;
}
</style>
