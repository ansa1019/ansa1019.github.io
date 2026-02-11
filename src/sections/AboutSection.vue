<template>
    <section :id="id" class="section about">
        <h1 class="about-title">
            <span class="typing beaver">{{ displayText }}</span>
            <span class="cursor beaver">|</span>
        </h1>
        <div class="about-desc">
            <h3>熟悉網頁開發，具備 Laravel 與 Django 實務經驗，能獨立完成切版、API 串接與系統架構設計。<br>具備 Python 數據整理與自動化處理能力，能將需求轉化為具體可行的小功能。</h3>
            <p>喜歡把複雜內容整理成清晰易用的介面，並樂於與團隊合作解決問題。碩士期間累積了網頁開發與資料分析的經驗，目前專注提升 TypeScript 與 Vue 3，期望加入能持續成長與挑戰的團隊。</p>
            <ul class="bullets">
                <li>網頁開發：Laravel / Django / Vue / TypeScript / SQL / RWD / AWS </li>
                <li>資料分析：Python / GPT API</li>
            </ul>
        </div>
        <div class="about-resume">
            <button class="resume-btn" @click="downloadResume">
                下載履歷
            </button>
        </div>
    </section>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue"
defineProps<{ id: string }>()

const messages = ["Hi~\n我是洪豆！"]

const displayText = ref("")
let index = 0, charIndex = 0

onMounted(() => typeText())

function typeText() {
    if (charIndex < messages[index].length) {
        displayText.value += messages[index].charAt(charIndex)
        charIndex++
        setTimeout(typeText, 80)
    } else {
        setTimeout(eraseText, 3000)
    }
}

function eraseText() {
    if (charIndex > 0) {
        displayText.value = messages[index].substring(0, charIndex - 1)
        charIndex--
        setTimeout(eraseText, 50)
    } else {
        index = (index + 1) % messages.length
        setTimeout(typeText, 500)
    }
}

const downloadResume = () => {
    window.open('/resume.pdf', '_blank')
}
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

.about-resume {
    text-align: center;
}

.about h3 {
    text-align: center;
    line-height: 2;
}

.typing {
    white-space: pre-line;
}

.cursor {
    display: inline-block;
    animation: blink 0.8s infinite;
}

.resume-btn {
    background-color: var(--second);
    color: #fff;
    padding: 0.75rem 1.5rem;
    border: none;
    border-radius: 8px;
    font-size: 1.1rem;
    cursor: pointer;
    transition: all 0.3s ease;
}

.resume-btn:hover {
    background-color: var(--primary);
    color: #fff;
    transform: translateY(2px);
}

@media (max-width: 640px) {
    .about-title {
        font-size: 4rem;
    }
}

@keyframes blink {
    50% {
        opacity: 0;
    }
}
</style>
