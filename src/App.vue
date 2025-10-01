<template>
  <header class="nav">
    <button class="menu-btn" @click="toggleMenu" :class="{ open: menuOpen }">
      <span></span>
      <span></span>
      <span></span>
    </button>

    <nav class="desktop-nav">
      <a v-for="item in items" :key="item.id" class="link" :href="`#${item.id}`"
        :class="{ active: activeSection === item.id }">
        {{ item.label }}
      </a>
    </nav>

    <div class="mobile-menu" :class="{ open: menuOpen }">
      <a v-for="item in items" :key="item.id" class="menu-link" :href="`#${item.id}`" @click="closeMenu">
        {{ item.label }}
      </a>
    </div>
  </header>

  <main class="content">
    <component v-for="item in items" :key="item.id" :is="sections[item.id]" :id="item.id" />
  </main>
</template>

<script setup lang="ts">
import { defineAsyncComponent, nextTick, onBeforeUnmount, onMounted, ref, watch, type Component } from 'vue';

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

const menuOpen = ref(false)
const toggleMenu = () => (menuOpen.value = !menuOpen.value)
const closeMenu = () => (menuOpen.value = false)

onMounted(async () => {
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
    { root: null, threshold: 0.3 }
  )

  await nextTick()
  setTimeout(() => {
    for (const i of items) {
      const el = document.getElementById(i.id)
      if (el) observer?.observe(el)
    }
  }, 100)
})

watch(menuOpen, (val) => {
  if (typeof document !== 'undefined') {
    document.body.style.overflow = val ? 'hidden' : 'auto'
    document.body.style.height = val ? '100vh' : 'auto'
  }
})

onBeforeUnmount(() => observer?.disconnect())

alert("👋 Hello！感謝您來訪！\n這個網站是我用來學習 Vue 的練習作品，還在努力完工中，請耐心等候～✨");
</script>



<style scoped>
.nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: var(--nav-h, 64px);
  z-index: 100;
  background: var(--nav-bg);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0 1.5rem;
  border-bottom: 1px solid rgba(0, 0, 0, .06);
  backdrop-filter: saturate(180%) blur(6px);
}

.desktop-nav {
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

.link.active::after {
  transform: translateX(-50%) scaleX(1) !important;
}

.menu-btn {
  position: absolute;
  left: 1.5rem;
  top: 50%;
  transform: translateY(-50%);
  width: 2.5rem;
  height: 1.75rem;
  display: none;
  flex-direction: column;
  justify-content: space-between;
  cursor: pointer;
  background: none;
  border: none;
  z-index: 50;
}

.menu-btn span {
  display: block;
  height: 3px;
  width: 100%;
  background: var(--primary);
  border-radius: 2px;
  transition: all 0.3s ease;
}

.menu-btn.open span:nth-child(1) {
  width: 2rem;
  transform: rotate(45deg) translate(1rem, 1rem);
}

.menu-btn.open span:nth-child(2) {
  opacity: 0;
}

.menu-btn.open span:nth-child(3) {
  width: 2rem;
  transform: rotate(-45deg) translate(0, 0);
}

.mobile-menu {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: var(--second);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 2rem;
  font-size: 2rem;
  transform: translateX(-100%);
  transition: transform .3s ease;
  z-index: 10;
  overflow: hidden;
}

.mobile-menu.open {
  transform: translateX(0);
}

.menu-link {
  position: relative;
  display: inline-block;
  text-decoration: none;
  color: var(--primary);
  text-transform: lowercase;
  padding: 8px 0;
  line-height: 1;
}

.menu-link::after {
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

.menu-link:hover::after {
  transform: translateX(-50%) scaleX(1);
}

.content {
  padding: calc(var(--nav-h) + 1rem) 4rem 1rem 4rem;
}

.section {
  min-height: calc(100vh - var(--nav-h));
  display: flex;
  flex-direction: column;
  justify-content: center;
  scroll-margin-top: calc(var(--nav-h) + 1rem);
}

.title {
  font-size: 2.5rem;
  line-height: 1.2;
}

.desc {
  opacity: 0.8;
}

@media (max-width: 640px) {
  .nav {
    height: var(--nav-m-h, 64px);
  }

  .content {
    padding: calc(var(--nav-m-h) + 1rem) 2rem 1rem 2rem;
  }

  .section {
    scroll-margin-top: calc(var(--nav-m-h) + 1rem);
  }

  .desktop-nav {
    display: none;
  }

  .menu-btn {display: flex;
  }
}
</style>
