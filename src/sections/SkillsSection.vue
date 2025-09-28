<template>
    <section :id="id" class="section skills">
        <h2 class="title">skills</h2>
        <div class="grid">
            <div v-for="(skill, i) in skills" :key="i" class="card" ref="circles">
                <div class="circle-wrapper">
                    <svg class="progress-ring" width="160" height="160">
                        <circle class="progress-ring__bg" stroke="#eee" stroke-width="12" fill="transparent" :r="radius"
                            :cx="80" :cy="80" />
                        <circle class="progress-ring__circle" :stroke="skill.color" stroke-width="12" fill="transparent"
                            :r="radius" :cx="80" :cy="80"
                            :style="{ strokeDasharray: circumference, strokeDashoffset: circumference }"
                            :data-progress="skill.progress" />
                    </svg>

                    <div class="circle-text">{{ skill.progress }}%</div>
                </div>
                <h3>{{ skill.title }}</h3>
                <ul>
                    <li v-for="(item, j) in skill.items" :key="j">{{ item }}</li>
                </ul>
            </div>
        </div>
    </section>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue"
defineProps<{ id: string }>()

const radius = 70
const circumference = 2 * Math.PI * radius

const skills = [
    {
        title: "Frontend",
        progress: 90,
        color: "#4CAF50",
        items: ["Vue", "HTML5 / CSS3", "JavaScript / TypeScript", "Bootstrap / Tailwind CSS", "RWD"]
    },
    {
        title: "Backend & Frameworks",
        progress: 75,
        color: "#2196F3",
        items: ["Django", "Laravel"]
    },
    {
        title: "Databases",
        progress: 70,
        color: "#FF9800",
        items: ["MySQL", "MongoDB", "PostgreSQL"]
    },
    {
        title: "Others",
        progress: 80,
        color: "#9C27B0",
        items: ["Git", "AWS", "Python", "GPT API"]
    }
]

const circles = ref<HTMLElement[]>([])

onMounted(() => {
    const observer = new IntersectionObserver(
        (entries) => {
            entries.forEach((entry) => {
                const circle = entry.target.querySelector(".progress-ring__circle") as SVGCircleElement
                if (!circle) return
                const progress = Number(circle.dataset.progress)

                if (entry.isIntersecting) {
                    const offset = circumference - (progress / 100) * circumference
                    circle.style.transition = "stroke-dashoffset 1.2s ease-out"
                    circle.style.strokeDashoffset = String(offset)
                } else {
                    circle.style.transition = "none"
                    circle.style.strokeDashoffset = String(circumference)
                }
            })
        },
        { threshold: 0.5 }
    )

    circles.value.forEach((el) => observer.observe(el))
})
</script>

<style scoped>
.grid {
    display: grid;
    gap: 2rem;
    margin: auto 0;
    grid-template-columns: 1fr;
}

.card {
    padding: 1rem;
    text-align: center;
}

.card ul {
    text-align: start;
    width: fit-content;
    margin: 0 5%;
}


.circle-wrapper {
    position: relative;
    width: 160px;
    height: 160px;
    margin: 0 auto 1rem;
}

.progress-ring__bg {
    stroke-linecap: round;
}

.progress-ring__circle {
    stroke-linecap: round;
    transform: rotate(-90deg);
    transform-origin: 50% 50%;
    stroke-dasharray: 314;
    stroke-dashoffset: 314;
}

.circle-text {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-weight: bold;
    font-size: 1.8rem;
}


@media (min-width: 601px) and (max-width: 1000px) {
    .grid {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (min-width: 1001px) {
    .grid {
        grid-template-columns: repeat(4, 1fr);
    }
}
</style>
