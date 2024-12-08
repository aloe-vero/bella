<template>
    <div class="parallax-background" ref="parallaxBackground">
        <div class="content">
            <slot></slot>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        imageUrl: {
            type: String,
            required: true,
        },
    },
    mounted() {
        window.addEventListener("scroll", this.handleScroll);
    },
    beforeDestroy() {
        window.removeEventListener("scroll", this.handleScroll);
    },
    methods: {
        handleScroll() {
            const scrollY = window.scrollY; // Get the vertical scroll position
            const parallaxEffect = scrollY * 0.5; // Adjust the multiplier for desired parallax intensity
            this.$refs.parallaxBackground.style.backgroundPosition = `center ${parallaxEffect}px`;
        },
    },
};
</script>

<style scoped>
.parallax-background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    background-image: url("");
    /* Empty for dynamic binding */
    background-size: cover;
    background-position: center 0;
    background-repeat: no-repeat;
    z-index: -1;
    will-change: background-position;
    /* Optimize for performance */
}

.content {
    position: relative;
    z-index: 1;
    color: white;
    text-align: center;
    padding-top: 50px;
}
</style>
