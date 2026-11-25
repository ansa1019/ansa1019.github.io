<template>
    <section :id="id" class="section skills">
        <h2 class="title">skills</h2>
        <div class="grid">
            <div v-for="(skill, i) in skills" :key="i" class="card">
                <h3>{{ skill.title }}</h3>
                <ul>
                    <li v-for="(item, j) in skill.items" :key="j">{{ item }}</li>
                </ul>
            </div>
        </div>
    </section>
</template>

<script setup lang="ts">
defineProps<{ id: string }>()

const skills = [
    {
        title: "Frontend",
        items: ["Vue", "HTML5 / CSS3", "JavaScript / TypeScript", "Bootstrap / Tailwind CSS", "RWD"]
    },
    {
        title: "Backend & Frameworks",
        items: ["Django", "Laravel"]
    },
    {
        title: "Databases",
        items: ["MySQL", "Microsoft SQL Server", "MongoDB", "PostgreSQL"]
    },
    {
        title: "Others",
        items: ["Git", "AWS", "Python", "GPT API", "PowerBuilder", "Genero"]
    }
]
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
