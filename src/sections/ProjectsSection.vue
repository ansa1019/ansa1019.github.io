<template>
  <section id="projects" class="section projects">
    <h2 class="title">Projects</h2>
    <div class="projects-grid">
      <a v-for="(project, index) in projects" :key="index" class="project-card" :href="project.url">
        <img :src="project.image" :alt="project.title" class="project-img" />
        <div class="project-content">
          <h3 class="project-title">{{ project.title }}</h3>
          <p class="project-desc">{{ project.description }}</p>
        </div>
        <div class="project-tags">
          <span v-for="(tag, i) in project.tags" :key="i" class="tag">
            # {{ tag }}
          </span>
        </div>
      </a>
    </div>
  </section>
</template>

<script setup lang="ts">
const projects = [
  {
    title: "女性論壇",
    description: "碩士期間參與的團隊專案，主要負責後端開發，包含API設計與即時聊天室建置，支援前端功能運作。平台是一個女性論壇，提供月經紀錄、發文、留言、點讚、分享等互動功能，並具備即時聊天室、後台管理與檢舉機制，以確保社群的便利性與安全性。此專案展現了我在後端架構設計、即時通訊開發與系統整合上的能力。",
    image: "FJU_SDRC.png",
    tags: ["Laravel", "Django", "MySQL", "WebSocket"],
    url: "#"
  },
  {
    title: "醫療系統",
    description: "於碩士期間獨立開發的研究專案，使用Laravel架構完成網站雛型與資料庫設計，並成功部署於AWS。平台設計為醫學相關應用，具備基礎的使用者操作介面與資料管理功能，後續可與醫學分析模組進行整合。本專案展現了我在網頁開發、資料庫規劃與雲端部署上的實作能力。",
    image: "treatment.png",
    tags: ["Laravel", "AWS", "MySQL"],
    url: "/treatment"
  },
]
</script>

<style scoped>
.projects-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: stretch;
  margin-top: 1.5rem;
  gap: 2rem;
}

.project-card {
  flex: 1 1 350px;
  max-width: 450px;
  background: #fff;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.25s ease, box-shadow 0.25s ease;
  display: flex;
  flex-direction: column;
  margin: 0 auto;
}

.project-card:hover {
  transform: translateY(-6px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
}

.project-img {
  width: 100%;
  height: 320px;
  object-fit: fill;
}

.project-content {
  padding: 1rem 1.5rem;
  flex-grow: 1;
}

.project-title {
  font-size: 1.25rem;
  margin-bottom: 0.5rem;
  color: var(--primary);
}

.project-desc {
  font-size: 0.95rem;
  text-align: justify;
  opacity: 0.85;
  line-height: 1.5;
  flex-grow: 1;
}

.project-tags {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 0.5rem;
  padding: 1rem 1.5rem;
  padding-top: 0;
}

.tag {
  background-color: var(--third);
  color: var(--primary);
  font-size: 0.85rem;
  padding: 0.3rem 0.75rem 0.5rem 0.75rem;
  border-radius: 9999px;
  transition: background 0.2s;
}

.tag:hover {
  color: var(--third);
  background-color: var(--second);
}

.projects a {
  text-decoration: none;
}

@media (min-width: 768px) {
  .projects-card {
    margin: 0;
  }
}
</style>
