<template>
    <div class="content overflow-hidden relative">
        <div class="absolute top-5 right-0 py-1 px-5 text-white shadow-lg">
            <p>{{ date }}</p>
        </div>
        <div id="horizontal-stack" class="stack flex flex-row gap-10 mt-10" :style="`transform: translateX(-${selectedHoriz * 10.5}rem);`">
            <div v-for="category, i in categories" :key="i" class="flex flex-col items-center flex-grow-0" 
                :class="i !== selectedHoriz ? 'opacity-60' : 'opacity-90'">
                <img :src="category.icon" alt="" :class="i === selectedHoriz ? 'scale-125' : ''">
                <p v-if="i === selectedHoriz">{{ category.title }}</p>
            </div>
        </div>
        <Transition>
            <div v-if="verticalToggled">
                <div id="vertical-stack" class="stack flex flex-col gap-4 mt-10"
                    :style="`transform: translateY(-${selectedVert * 9}rem);`">
                    <div v-for="section, i in categories[selectedHoriz].sections" :key="i"
                        class="flex flex-row gap-5 items-center flex-grow-0 transition duration-200" 
                        :class="i !== selectedVert ? 'opacity-60': 'opacity-90'"
                        :style="`transform: translateY(-${i < selectedVert ? 12 : 0}rem);`"
                    >
                        <img :src="section.icon || '/ico/Icons.39.png'" class="transition duration-500" :class="i === selectedVert ? 'scale-125' : ''">
                        <div class="flex flex-col">
                            <p class="font-bold" :class="{ 'glow text-white text-2xl': i === selectedVert }">{{ section.title }}</p>
                            <p v-if="section.subtitle" class="text-lg">{{ section.subtitle }}</p>
                        </div>
                    </div>
                </div>
            </div>
        </Transition>
        <video autoplay muted loop id="background-video">
            <source src="/res/wave/blue.mp4" type="video/mp4">
        </video>
    </div>
</template>

<script setup>
import 'joypad.js';

let categories = [
    {
        title: "Users",
        icon: "/ico/Icons.6.png",
        sections: [
            {
                title: "John",
                icon: "/ico/Icons.9.png",
            },
            {
                title: "Ryan",
                icon: "/ico/Icons.9.png",
            },
            {
                title: "Albedo",
                icon: "/ico/Icons.9.png",
            },
            {
                title: "Jeffrey",
                icon: "/ico/Icons.9.png",
            },
            {
                title: "Bill Gates",
                icon: "/ico/Icons.9.png",
            },
        ]
    },
    {
        title: "Settings",
        icon: "/ico/Icons.35.png",
        sections: [
            {
                title: "System Update",
                subtitle: "Update your firmware",
                icon: "/ico/Icons.17.png",
            },
            {
                title: "Game Settings",
                subtitle: "Adjust settings for games",
                icon: "/ico/Icons.27.png",
            },
            {
                title: "Video Settings",
                subtitle: "Adjust settings for videos",
                icon: "/ico/Icons.67.png",
            },
            {
                title: "Music Settings",
                subtitle: "Adjust settings for songs",
                icon: "/ico/Icons.29.png",
            },
            {
                title: "Chat Settings",
                subtitle: "Adjust settings for chatting",
                icon: "/ico/Icons.24.png",
            },
            {
                title: "System Settings",
                subtitle: "Adjust settings for PlayStation 3 System",
                icon: "/ico/Icons.36.png",
            },
            {
                title: "Theme Settings",
                subtitle: "Adjust your themes",
                icon: "/ico/Icons.23.png",
            },
        ]
    },
    {
        title: "Photo",
        icon: "/ico/Icons.1.png",
        sections: [
            {
                title: "Empty Album",
                subtitle: "0 photos",
                icon: "/ico/Icons.8.png",
            },
        ]
    },
    {
        title: "Songs",
        icon: "/ico/Icons.16.png",
        sections: [
            {
                title: "Empty Album",
                subtitle: "0 songs",
                icon: "/ico/Icons.8.png",
            },
        ]
    },
    {
        title: "Movies",
        icon: "/ico/Icons.38.png",
        sections: [
            {
                title: "Empty Collection",
                subtitle: "0 movies",
                icon: "/ico/Icons.8.png",
            },
        ]
    },
    {
        title: "Game",
        icon: "/ico/Icons.39.png",
        sections: [
            {
                title: "The Last of Us",
                subtitle: `Last Played: ${(Math.random() * 10).toFixed(0)} days ago`,
                icon: "/ico/Icons.63.png",
            },
            {
                title: "Journey",
                subtitle: `Last Played: ${(Math.random() * 10).toFixed(0)} days ago`,
                icon: "/ico/Icons.63.png",
            },
            {
                title: "Red Dead Redemption",
                subtitle: `Last Played: ${(Math.random() * 10).toFixed(0)} days ago`,
                icon: "/ico/Icons.63.png",
            },
            {
                title: "Grand Theft Auto V",
                subtitle: `Last Played: ${(Math.random() * 10).toFixed(0)} days ago`,
                icon: "/ico/Icons.63.png",
            },
            {
                title: "Grand Theft Auto IV",
                subtitle: `Last Played: ${(Math.random() * 10).toFixed(0)} days ago`,
                icon: "/ico/Icons.63.png",
            },
            {
                title: "Bioshock Infinite",
                subtitle: `Last Played: ${(Math.random() * 10).toFixed(0)} days ago`,
                icon: "/ico/Icons.63.png",
            },
            {
                title: "Uncharted 2: Among Thieves",
                subtitle: `Last Played: ${(Math.random() * 10).toFixed(0)} days ago`,
                icon: "/ico/Icons.63.png",
            },
        ]
    },
    {
        title: "Network",
        icon: "/ico/Icons.18.png"
    },
    {
        title: "Friends",
        icon: "/ico/Icons.9.png",
        sections: [
            {
                title: "No friends",
                subtitle: "Wait, you thought I had friends?",
                icon: "/ico/Icons.9.png",
            },
            {
                title: "Add friends",
                subtitle: "If you ever get any",
                icon: "/ico/Icons.11.png",
            }
        ]
    }
]

function getDateStr() {
    let date = new Date();
    let month = date.getUTCMonth() + 1;
    let day = date.getDay() + 1;
    let hours = date.getHours();
    let mins = date.getMinutes();
    if (mins < 10) {
        mins = `0${mins}`;
    }
    return `${month}/${day} ${hours}:${mins} 🕑`
}
let date = $ref(getDateStr());
setInterval(() => {
    date = getDateStr();
}, 1000);

function clamp(num, min, max) {
    return Math.min(Math.max(num, min), max);
};

let selectedHoriz = $ref(0);
let selectedVert = $ref(0);
let verticalToggled = $ref(true);

document.onkeydown = (e) => {
    let keyMap = [
        { keys: ["arrowleft", "a"], action: left },
        { keys: ["arrowright", "d"], action: right },
        { keys: ["arrowup", "w"], action: up },
        { keys: ["arrowdown", "s"], action: down }
    ]

    for (let binding of keyMap) {
        if (binding.keys.includes(e.key.toLowerCase())) {
            binding.action();
            break;
        }
    }
}

joypad.on('connect', e => {
    const { id } = e.gamepad;
    console.log(`${id} connected!`);
});

joypad.on('button_press', e => {
    const { buttonName } = e.detail;
    if (buttonName === "button_4") {
        left();
    } 
    if (buttonName === "button_5") {
        right();
    }

    //console.log(`${buttonName} was pressed!`);
});


function left() {
    if (selectedHoriz === 0) return;
    verticalToggled = false;
    selectedHoriz = clamp(--selectedHoriz, 0, categories.length - 1);
    if (selectedVert > categories[selectedHoriz].sections.length - 1) {
        selectedVert = categories[selectedHoriz].sections.length - 1;
    }
    setInterval(() => {
        verticalToggled = true;
    }, 200)
}

function right() {
    if (selectedHoriz === categories.length - 1) return;
    verticalToggled = false;
    selectedHoriz = clamp(++selectedHoriz, 0, categories.length - 1);
    if (selectedVert > categories[selectedHoriz].sections.length - 1) {
        selectedVert = categories[selectedHoriz].sections.length - 1;
    }
    setInterval(() => {
        verticalToggled = true;
    }, 200)
}

function up() {
    selectedVert = clamp(--selectedVert, 0, categories[selectedHoriz].sections.length - 1);
}

function down() {
    selectedVert = clamp(++selectedVert, 0, categories[selectedHoriz].sections.length - 1);
}

</script>

<style>

.glow {
    color: #fff;
    -webkit-animation: glow 1s ease-in-out infinite alternate;
    -moz-animation: glow 1s ease-in-out infinite alternate;
    animation: glow 1s ease-in-out infinite alternate;
}

@keyframes glow {
  from {
    @apply brightness-90;
    text-shadow: 0 0 10px #fff, 0 0 20px #fff
  }
  to {
    @apply brightness-100;
    text-shadow: 0 0 20px #fff, 0 0 20px #ffffff, 0 0 35px #ffffff
  }
}

#background-video {
    width: 100vw;
    height: 100vh;
    object-fit: cover;
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    z-index: -1;
}

body {
    background: url('/ico/b_HD.jpg');
    /*background: url('/res/ps3.mp4');*/
    color: rgb(200, 200, 200);
    @apply text-xl overflow-hidden;
}

.content {
    padding-left: 20vw;
    padding-top: 10vw;
}

.stack {
    transition-property: transform;
    transition-duration: 200ms;
}

.v-enter-active,
.v-leave-active {
  transition: all 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>