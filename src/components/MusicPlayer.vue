<template>
    <div class="music-player">
        <!-- Album Image -->
        <div class="album-image">
            <img src="https://pbs.twimg.com/media/EeFdxZ2U0AU91-3?format=jpg&name=large" alt="Album Art" />
        </div>

        <!-- Controls -->
        <div class="control-panel">
            <div class="controls">
                <!-- Previous Song Button -->
                <button @click="previousSong">
                    <span class="icon">&#9664;</span> <!-- Left Arrow -->
                </button>

                <!-- Play/Pause Toggle Button -->
                <button @click="togglePlayPause">
                    <span class="icon">
                        <span v-if="isPlaying">&#10074;&#10074;</span> <!-- Pause Icon -->
                        <span v-else>&#9654;</span> <!-- Play Icon -->
                    </span>
                </button>

                <!-- Next Song Button -->
                <button @click="nextSong">
                    <span class="icon">&#9654;</span> <!-- Right Arrow -->
                </button>
            </div>
        </div>

        <!-- Hidden YouTube Player -->
        <div id="youtubePlayer"></div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            albumImage: "https://pbs.twimg.com/media/EeFdxZ2U0AU91-3?format=jpg&name=large",
            youtubePlaylist: "PLnGXb33MhhGCXQwKswwCnAkgRNQLq0OAm", // Replace with your YouTube playlist ID
            isPlaying: false, // Track whether the player is playing
            player: null, // YouTube player instance
        };
    },
    methods: {
        initializePlayer() {
            this.player = new YT.Player("youtubePlayer", {
                height: "0",
                width: "0",
                playerVars: {
                    listType: "playlist",
                    list: this.youtubePlaylist,
                    autoplay: 1,
                    modestbranding: 1,
                    rel: 0,
                    mute: 1, // Start muted to allow autoplay in Chrome
                },
                events: {
                    onReady: this.onPlayerReady,
                    onStateChange: this.onPlayerStateChange,
                },
            });
        },
        onPlayerReady(event) {
            console.log("Player is ready.");
            // Autoplay while muted
            event.target.playVideo();

            // Set a reduced volume after unmuting
            setTimeout(() => {
                event.target.unMute(); // Unmute
                event.target.setVolume(20); // Set volume to 20%
            }, 1000); // Wait 1 second to ensure the player starts properly
        },
        onPlayerStateChange(event) {
            switch (event.data) {
                case YT.PlayerState.PLAYING:
                    this.isPlaying = true;
                    break;
                case YT.PlayerState.PAUSED:
                    this.isPlaying = false;
                    break;
                case YT.PlayerState.ENDED:
                    this.nextSong();
                    break;
            }
        },
        togglePlayPause() {
            if (!this.player || typeof this.player.playVideo !== "function") {
                console.error("Player is not ready yet.");
                return;
            }
            if (this.isPlaying) {
                this.player.pauseVideo();
            } else {
                this.player.playVideo();
            }
        },
        nextSong() {
            if (this.player) this.player.nextVideo();
        },
        previousSong() {
            if (this.player) this.player.previousVideo();
        },
    },
    mounted() {
        if (!window.YT) {
            const tag = document.createElement("script");
            tag.src = "https://www.youtube.com/iframe_api";
            const firstScriptTag = document.getElementsByTagName("script")[0];
            firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

            window.onYouTubeIframeAPIReady = this.initializePlayer;
        } else {
            this.initializePlayer();
        }
    },
    beforeUnmount() {
        if (this.player) this.player.destroy();
    },
};
</script>


<style scoped>
/* Center the music player in the viewport */
.music-player {
    position: fixed;
    top: 60%;
    /* Center vertically */
    left: 50%;
    /* Center horizontally */
    transform: translate(-50%, -50%);
    /* Adjust for the element's size */
    width: 300px;
    padding: 16px;
    display: flex;
    flex-direction: column;
    align-items: center;
    border-radius: 8px;
    background-color: rgba(255, 184, 225, 0.5);
    /* Semi-transparent background */
    box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
    z-index: 1000;
    /* Keeps it above other elements */
}

/* Album Image */
.album-image img {
    height: 200px;
    width: 200px;
    border-radius: 4px;
    object-fit: cover;
}

/* Control Panel */
.control-panel {
    margin-top: 16px;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
}

/* Controls Layout */
.controls {
    display: flex;
    justify-content: space-around;
    width: 100%;
}

/* Button Styles */
.controls button {
    background: none;
    border: none;
    font-size: 20px;
    cursor: pointer;
    padding: 8px 16px;
    border-radius: 4px;
    transition: background-color 0.3s, transform 0.2s;
}

/* Hover Effect for Buttons */
.controls button:hover {
    background-color: #ddd;
    transform: scale(1.1);
}

/* Icon Styling */
.icon {
    font-size: 20px;
    color: white;
}
</style>
