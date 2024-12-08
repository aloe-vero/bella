<template>
    <v-app>
        <div>
            <!-- Static Background Image -->
            <div class="background-image"></div>

            <!-- Scrollable Content -->
            <div class="scroll-container">
                <div v-for="(content, index) in containers" :key="index" class="container-wrapper" :class="{
                    'left-side': index % 2 === 0,
                    'right-side': index % 2 !== 0,
                }">
                    <div class="scroll-container-content">

                        <p>{{ content.text }}</p>
                    </div>
                </div>
            </div>
        </div>
    </v-app>
</template>

<script>
import background from "@/assets/background.jpeg";

export default {
    data() {
        return {
            backgroundImage: background, // Static background image
            containers: [
                {

                    text: "I hope you like this little fast gift i made for you.",
                },
                {

                    text: "My dear friend another year I’m gonna celebrate your birthday even if we live far from each other. I’m gonna make a big toast to you. I love that you live at the same time as me and it was a gift meeting you through the web. I couldn’t really think I could make a friend like but you prove me wrong. ",
                },
                {

                    text: "So, first of all, I just wanna say that I know you suffer a lot with your body image, because body dysphoria is a bitch, which I can relate to it. I know that voice is louder than mine, but you are beautiful the way you are. You’re beautiful! Like I wish I could beat that body dysmorphia out of you and make it suffer, so you don’t have to suffer.",
                },
                {

                    text: "Also, it was really great finally meeting you in person. I was really shy because well it was the first time meeting in person and also you’re really pretty so yep!. But I hope we can meet more often and get comfortable with each other and talk like we talk online. ",
                },
                {

                    text: "Happy Birthday ! I adore you so much ! ",
                }, {

                    text: "I adore you so much ! ",
                },
            ],
        };
    },
    mounted() {
        this.handleScroll(); // Trigger on mount to handle visible containers initially
        window.addEventListener("scroll", this.handleScroll);
    },
    methods: {
        handleScroll() {
            const elements = document.querySelectorAll(".container-wrapper");
            const windowHeight = window.innerHeight;

            elements.forEach((el) => {
                const rect = el.getBoundingClientRect();
                if (rect.top <= windowHeight * 0.8) {
                    el.classList.add("scroll-active");
                } else {
                    el.classList.remove("scroll-active");
                }
            });
        },
    },
    beforeDestroy() {
        window.removeEventListener("scroll", this.handleScroll);
    },
};
</script>
<style scoped>
/* Background Image */
.background-image {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    background-image: url("@/assets/background.jpeg");
    background-size: cover;
    background-position: center;
    z-index: -1;
}

/* Scroll Container */
.scroll-container {
    position: relative;
    padding-top: 100vh;
    padding-bottom: 50vh;
}

/* Individual Container Wrappers */
.container-wrapper {
    position: relative;
    margin: 50px 0;
    width: 100%;
    display: flex;

    justify-content: flex-start;
    opacity: 0;
    /* Initially hidden */
    transform: translateY(20px);
    /* Initial position for animation */
    transition: opacity 1s, transform 1s;
}

.container-wrapper.right-side {
    justify-content: flex-end;
    right: 10%;
}

.container-wrapper.left-side {
    justify-content: flex-start;
    left: 10%;
}

/* Container Content - Simplified Design */
.scroll-container-content {
    background: rgba(255, 255, 255, 0.8);
    /* Semi-transparent white background */
    padding: 20px;
    border-radius: 8px;
    /* Simple rounded corners */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    width: 300px;
    text-align: center;
    transition: box-shadow 0.5s ease-in-out;
    box-shadow: 0 0 20px rgba(255, 255, 255, 0.9), 0 0 40px rgba(235, 52, 192, 0.7);
}

/* Glow Effect */


/* Active Scroll Animation */
.container-wrapper.scroll-active {
    opacity: 1;
    transform: translateY(0);
}
</style>
