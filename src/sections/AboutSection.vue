<template>
    <section :id="id" class="section about">
        <h1 class="title beaver">Hi~<br>我是洪豆!</h1>
        <div class="about-desc">
            <h3>熟悉 HTML / CSS / JavaScript，能獨立切版與串接 API；也有 Python 數據整理經驗，擅長把需求轉成可落地的小功能。</h3>
            <p>我喜歡把複雜的內容整理成清楚的介面，也樂於與團隊一起解決問題。過去在碩士期間，累積了網頁開發的實務經驗。目前專注提升 TypeScript 與
                Vue 3 的經驗，希望投入能持續成長的團隊。</p>
            <ul class="bullets">
                <li>網頁開發：Laravel / Vue / TypeScript / Django / SQL / RWD / AWS </li>
                <li>資料分析：Python / GPT API</li>
            </ul>
        </div>
    </section>
</template>

<script setup lang="ts">
defineProps<{ id: string }>()
</script>

<style scoped>
.title {
    font-size: 5rem;
    margin: 3rem 0 6rem 0;
}

.bullets {
    margin: 0;
    padding-left: 1.2rem;
    display: grid;
    gap: .5rem
}

.about-desc {
    padding: 0 1rem;
}

h3 {
    text-align: center;
}
</style>
