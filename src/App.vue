<template>
  <header class="nav">
    <nav>
      <a v-for="item in items" :key="item.id" class="link" :href="`#${item.id}`"
        :class="{ active: activeSection === item.id }">
        {{ item.label }}
      </a>
    </nav>
  </header>

  <main class="content">
    <component v-for="item in items" :key="item.id" :is="sections[item.id]" :id="item.id" />
  </main>
</template>

<script setup lang="ts">
import { defineAsyncComponent, type Component, onMounted, onBeforeUnmount, ref } from 'vue'

type SectionId = 'about' | 'skills' | 'experience' | 'projects' | 'contact'
const items: { id: SectionId; label: string }[] = [
  { id: 'about', label: 'about' },
  { id: 'skills', label: 'skills' },
  { id: 'experience', label: 'experience' },
  { id: 'projects', label: 'projects' },
  { id: 'contact', label: 'contact' }
]
const sections: Record<SectionId, Component> = {
  about: defineAsyncComponent(() => import('@/sections/AboutSection.vue')),
  skills: defineAsyncComponent(() => import('@/sections/SkillsSection.vue')),
  experience: defineAsyncComponent(() => import('@/sections/ExperienceSection.vue')),
  projects: defineAsyncComponent(() => import('@/sections/ProjectsSection.vue')),
  contact: defineAsyncComponent(() => import('@/sections/ContactSection.vue')),
}
const activeSection = ref<SectionId>('about')
let observer: IntersectionObserver | null = null

onMounted(() => {
  if (typeof window === 'undefined' || !('IntersectionObserver' in window)) {
    activeSection.value = items[0].id
    return
  }

  observer = new IntersectionObserver(
    (entries: IntersectionObserverEntry[]) => {
      for (const e of entries) {
        if (e.isIntersecting) {
          activeSection.value = (e.target as HTMLElement).id as SectionId
        }
      }
    },
    { root: null, rootMargin: '-40% 0px -50% 0px', threshold: 0.01 }
  )

  for (const i of items) {
    const el = document.getElementById(i.id)
    if (el) observer.observe(el)
  }
})

onBeforeUnmount(() => observer?.disconnect())
</script>


<style scoped>
.nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: var(--nav-h, 64px);
  z-index: 10;
  background: var(--nav-bg);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0 1.5rem;
  border-bottom: 1px solid rgba(0, 0, 0, .06);
  backdrop-filter: saturate(180%) blur(6px);
}

nav {
  display: flex;
  gap: 1.5rem;
  font-size: 1.25rem;
}


.link {
  position: relative;
  display: inline-block;
  text-decoration: none;
  color: var(--primary);
  text-transform: lowercase;
  padding: 8px 0;
  line-height: 1;
}

.link::after {
  content: "";
  position: absolute;
  left: 50%;
  bottom: 2px;
  width: 100%;
  height: 2px;
  background: currentColor;
  transform: translateX(-50%) scaleX(0);
  transform-origin: center;
  transition: transform .28s ease;
  pointer-events: none;
}

.link:hover::after {
  transform: translateX(-50%) scaleX(1);
}


.content {
  padding: calc(var(--nav-h) + 1rem) 4rem 1rem 4rem;
}

.section {
  min-height: 60vh;
  display: grid;
  align-content: start;
  gap: 1rem;
  scroll-margin-top: calc(var(--nav-h) + 1rem);
}

.title {
  font-size: 2.5rem;
  line-height: 1.2;
}

.desc {
  opacity: 0.8;
}
</style>
