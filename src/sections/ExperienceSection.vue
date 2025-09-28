<template>
  <section :id="id" class="section experience">
    <h2 class="title">experience</h2>
    <div class="timeline">
      <div v-for="(item, i) in exp" :key="i" class="timeline-item" :class="{ left: i % 2 === 1, right: i % 2 === 0 }"
        ref="items">
        <div class="timeline-dot">{{ item.icon }}</div>
        <div class="timeline-date">{{ item.range }}</div>
        <h3 class="timeline-title">{{ item.title }}</h3>
        <h4 class="timeline-subtitle">{{ item.subtitle }}</h4>
        <p class="timeline-desc" v-html="item.desc"></p>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue"
defineProps<{ id: string }>()

const exp = [
  {
    icon:'🎓',
    title: '碩士',
    subtitle: '國立屏東科技大學 資訊管理系',
    range: '2023/09 – 2025/06',
    desc: '碩士論文研究：\n設計並實作自動化文本分析流程，使用 <b>Python</b> 進行大規模資料處理與統計建模，結合 <b>爬蟲技術</b> 蒐集上市公司年報與財務資料，並透過 <b>OpenAI GPT API</b> 建立摘要與語意分析管線。後續以 <b>資料分析</b> 與迴歸模型驗證資訊科技能力對組織韌性的影響，熟悉資料清理、特徵萃取、語意相似度計算及統計驗證等技術。\n網站開發專案：\n1. 團隊專案：接手學長姐延續專案，3人組成團隊，主要負責 工作分配 與 100% 後端 <b>Django</b> 開發，並支援約 30% <b>Laravel</b> 前端，確保專案順利交付。\n2. 獨立專案：獨立完成全端系統開發，涵蓋 需求分析、<b>資料庫設計</b>、API 串接與前後端整合，並負責 <b>AWS</b> 部署與維運（伺服器環境建置、資料庫管理與雲端架構設定），成功確保系統穩定上線與持續運行。'
  },
  {
    icon:'💼',
    title: '助理工程師',
    subtitle: '燁聯鋼鐵股份有限公司',
    range: '2023/02 – 2023/06',
    desc: '於大四實習參與公司內部系統轉換，負責以 <b>Genero</b> 獨立規劃並撰寫新採購系統，涵蓋報表輸出、資料處理及基礎管理模組。透過程式優化與流程重構，系統效率提升約 30%，日常報表產出速度加快 10%，顯著減少人工作業時間，並展現需求分析與完整系統開發能力。'
  }, {
    icon:'🎓',
    title: '學士',
    subtitle: '國立屏東科技大學 資訊管理系',
    range: '2019/09 – 2023/06',
    desc: '以 <b>Arduino</b> 與低成本光譜感測器建構植物生長監測系統，能即時收集並傳輸數據至 <b>即時監測平台</b>，提供可視化的生長狀況資訊。專案中結合 <b>機器學習</b> 對植物生長及水分狀態進行分類與預測，並進一步生成澆水建議，完整展現硬體整合、即時資料處理、前端視覺化與 AI 模型應用的能力。'
  }
]

const items = ref<HTMLDivElement[]>([])

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("visible")
        } else {
          entry.target.classList.remove("visible") // 👈 滑出時移除
        }
      })
    },
    { threshold: 0.3 }
  )
  items.value.forEach((el) => observer.observe(el))
})
</script>

<style scoped>
.timeline {
  position: relative;
  padding-top: 2.5rem;
}

.timeline::before {
  content: "";
  position: absolute;
  top: 0;
  bottom: 0;
  left: 50%;
  width: 3px;
  background: #ccc;
  transform: translateX(-50%);
  z-index: 1;
}

.timeline-item {
  position: relative;
  min-height: 250px;
  width: 50%;
  padding: 1rem 2rem;
  opacity: 0;
  transition: opacity .6s ease, transform .6s ease;
  z-index: 2;
}

.timeline-item.left {
  left: 0;
  transform: translateX(-60px);
  text-align: right;
}

.timeline-item.right {
  left: 50%;
  transform: translateX(60px);
  text-align: left;
}

.timeline-item.visible {
  opacity: 1;
  transform: translateX(0);
}

.timeline-dot {
  position: absolute;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: #fff;
  border: 3px solid var(--primary);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  z-index: 3;
}

.timeline-item.left .timeline-dot {
  left: 100%;
}

.timeline-item.right .timeline-dot {
  left: 0;
  transform: translateX(-50%);
}

.timeline-date {
  margin: 0 0 0.75rem;
  border-radius: 20px;
  font-weight: bold;
  display: inline-block;
  margin-bottom: 0.5rem;
}

.timeline-title {
  margin: 0 0 0.75rem;
  font-weight: bold;
}

.timeline-subtitle {
  margin: 0 0 0.75rem;
  font-size: 0.9rem;
  opacity: .7;
}

.timeline-desc {
  margin: 0;
  font-size: 0.9rem;
  line-height: 1.4;
  white-space: pre-line;
  text-align: justify;
}

@media (max-width: 640px) {
  .timeline::before {
    left: 1rem;
  }

  .timeline-item {
    width: 100%;
    left: 1rem !important;
    text-align: left !important;
  }

  .timeline-icon {
    transform: translateX(0);
  }

  .timeline-content {
    margin-left: 3rem;
  }

  .timeline-item.left .timeline-dot {
  left: 0;
}

.timeline-item.right .timeline-dot {
  left: 0;
  transform: translateX(-50%);
}
}
</style>
