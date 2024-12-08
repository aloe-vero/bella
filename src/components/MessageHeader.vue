<template>
    <div v-if="isVisible" class="message-container">
        <transition-group name="fade" tag="div" class="message-list">
            <div v-for="(message, index) in displayedMessages" :key="index" class="message-text">
                {{ message }}
            </div>
        </transition-group>
    </div>
</template>

<script>
export default {
    data() {
        return {
            messages: [
                "Salut! Ma belle Isabelle",
                "Aujourd'hui, c'est une belle journée!",
                "I'm happy you are here, ",
                "even though we live far from each other "
            ],
            displayedMessages: [], // Tracks messages currently displayed
            currentIndex: 0, // Index of the message to be displayed next
            isVisible: true, // Controls visibility based on scroll position
        };
    },
    mounted() {
        this.showMessages();
        window.addEventListener("scroll", this.handleScroll);
    },
    beforeDestroy() {
        window.removeEventListener("scroll", this.handleScroll);
    },
    methods: {
        showMessages() {
            const displayNextMessage = () => {
                if (this.currentIndex < this.messages.length) {
                    this.displayedMessages.push(this.messages[this.currentIndex]);
                    this.currentIndex++;
                    setTimeout(displayNextMessage, 3000); // Delay between each message
                }
            };

            displayNextMessage();
        },
        handleScroll() {
            const currentScrollY = window.scrollY;

            // Show only when near the top of the page
            if (currentScrollY < 50) {
                this.isVisible = true;
            } else {
                this.isVisible = false;
            }
        },
    },
};
</script>

<style scoped>
/* Container for all messages */
.message-container {
    position: fixed;
    top: 5%;
    /* Adjust this value to move messages closer to the top */
    left: 50%;
    transform: translateX(-50%);
    z-index: 1000;
    text-align: center;
    font-family: Arial, sans-serif;
}

/* List of messages */
.message-list {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 16px;
    /* Space between messages */
}

/* Individual message text */
.message-text {
    font-size: 36px;
    font-weight: bold;
    color: white;
    opacity: 0;
    animation: fadeIn 1s ease-in-out forwards;
}

/* Fade-in animation */
@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(20px);
    }

    to {
        opacity: 1;
        transform: translateY(0);
    }
}

/* Fade transition for Vue's transition-group */
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s ease-in-out, transform 0.5s ease-in-out;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
    transform: translateY(20px);
}
</style>
