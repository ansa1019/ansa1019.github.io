<template>
    <section :id="id" class="section about">
        <h1 class="about-title beaver">Hi~<br>我是洪豆!</h1>
        <div class="about-desc">
            <h3>熟悉網頁開發，具備 Laravel 與 Django 實務經驗，能獨立完成切版、API 串接與系統架構設計。<br>具備 Python 數據整理與自動化處理能力，能將需求轉化為具體可行的小功能。</h3>
            <p>喜歡把複雜內容整理成清晰易用的介面，並樂於與團隊合作解決問題。碩士期間累積了網頁開發與資料分析的經驗，目前專注提升 TypeScript 與 Vue 3，期望加入能持續成長與挑戰的團隊。</p>
            <ul class="bullets">
                <li>網頁開發：Laravel / Django / Vue / TypeScript / SQL / RWD / AWS </li>
                <li>資料分析：Python / GPT API</li>
            </ul>
        </div>
    </section>
</template>

<script setup lang="ts">
defineProps<{ id: string }>()
</script>

<style scoped>
.bullets {
    margin: 0;
    padding-left: 1.2rem;
    display: grid;
    gap: .5rem
}

.about-title {
    text-align: center;
    font-size: 5rem;
    margin: 3rem 0 6rem 0;
}

.about-desc {
    max-width: 1000px;
    margin: 0 auto;
    padding: 0 1rem;
    text-align: justify;
}

.about h3 {
    text-align: center;
    line-height: 2;
}

@media (max-width: 640px) {
    .about-title {
        font-size: 4rem;
    }
}
</style>
