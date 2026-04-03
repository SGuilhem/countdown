<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

// ── CONFIG ──────────────────────────────────────────────────
const TARGET_DATE = new Date('2026-04-03T17:00:00')

const SURPRISE_ADDRESS = `12 rue de la Surprise\n75001 Paris`

const PHRASES = [
  'Tu veux bien me rejoindre ce soir? :^)',
  "Bon y'a peut-être pas des phrases à l'infini...",
  'Tu es la meilleure binome de karaoké au monde',
  'Tu es capable de faire tant de chose grâce à ta volonté',
  'Tu es la plus belle et la plus précieuse à mes yeux',
  "Tu vaux le coup tous les jours, je t'aime",
  "Tu as la taille parfaite pour t'attraper et d'amener n'importe où (dans la chambre)",
  "Voyager avec toi à l'autre bout du monde est une des meilleures choses que j'ai faites",
  "J'aime ton rire quand tu te gosses sur une blague nulle, même quand tu fais le cochon",
  "T'es sacrément fineaude aussi, j'adore ça",
  "J'aime tous tes grains de beauté (du visage jusqu'à ton orteil, en passant par tes fesses)",
  "T'as changé ma vie, et je veux continuer à la changer avec toi. Je t'aime fort",
  'Hello hello Sexy Razowsky',
  "J'espère que tu vas avoir dans la tête 'GIVE ME COMPLIMENTS, GIVE ME COMPLIMENTS!''",
  "With a taste of your lips, I'm on a ride",
]

const PHOTOS = [
  '/photos/photo1.jpeg',
  '/photos/photo2.jpeg',
  '/photos/photo3.jpeg',
  '/photos/photo4.jpeg',
  '/photos/photo5.jpeg',
  '/photos/photo6.jpeg',
  '/photos/photo7.jpeg',
  '/photos/photo8.jpeg',
  '/photos/photo9.jpeg',
  '/photos/photo10.jpeg',
  '/photos/photo11.jpeg',
  '/photos/photo12.jpeg',
  '/photos/photo13.jpeg',
  '/photos/photo14.jpeg',
  '/photos/photo15.jpeg',
  '/photos/photo16.jpeg',
  '/photos/photo17.jpeg',
  '/photos/photo18.jpeg',
  '/photos/photo19.jpeg',
  '/photos/photo20.jpeg',
  '/photos/photo21.jpeg',
  '/photos/photo22.jpeg',
  '/photos/photo23.jpeg',
]
// ────────────────────────────────────────────────────────────

const randomPhrase = PHRASES[Math.floor(Math.random() * PHRASES.length)]
const randomPhoto = PHOTOS[Math.floor(Math.random() * PHOTOS.length)]

const now = ref(new Date())
let timer = null

onMounted(() => {
  timer = setInterval(() => {
    now.value = new Date()
  }, 1000)
})
onUnmounted(() => clearInterval(timer))

const diff = computed(() => Math.max(0, TARGET_DATE - now.value))

const isFinished = computed(() => diff.value === 0)

const days = computed(() => Math.floor(diff.value / 86400000))
const hours = computed(() => Math.floor((diff.value % 86400000) / 3600000))
const minutes = computed(() => Math.floor((diff.value % 3600000) / 60000))
const seconds = computed(() => Math.floor((diff.value % 60000) / 1000))

const pad = (n) => String(n).padStart(2, '0')
</script>

<template>
  <!-- Fond photo plein écran -->
  <div
    class="relative min-h-screen w-full flex items-center justify-center overflow-hidden bg-black"
  >
    <!-- Photo de fond -->
    <img :src="randomPhoto" alt="" class="absolute inset-0 w-full h-full object-contain" />
    <!-- Overlay sombre -->
    <div class="absolute inset-0 bg-black/50" />

    <!-- Contenu centré, positionné à 3/4 de la hauteur -->
    <div class="absolute z-10 left-1/2 -translate-x-1/2" style="top: 7%">
      <div
        class="flex flex-col items-center gap-2 px-6 py-4 text-center text-white rounded-2xl"
        style="
          backdrop-filter: blur(12px);
          background: rgba(0, 0, 0, 0.35);
          border: 1px solid rgba(255, 255, 255, 0.15);
        "
      >
        <p
          v-if="!isFinished"
          class="text-lg md:text-2xl font-light italic tracking-wide drop-shadow"
        >
          Bonjour Gaby Queen!
        </p>
        <p class="text-base md:text-lg">{{ randomPhrase }}</p>

        <div v-if="!isFinished" class="flex gap-3 md:gap-6">
          <div
            v-for="{ value, label } in [
              { value: pad(days), label: 'jours' },
              { value: pad(hours), label: 'heures' },
              { value: pad(minutes), label: 'minutes' },
              { value: pad(seconds), label: 'secondes' },
            ]"
            :key="label"
            class="flex flex-col items-center"
          >
            <span class="text-4xl md:text-7xl font-bold tabular-nums drop-shadow-lg leading-none">
              {{ value }}
            </span>
            <span class="text-xs uppercase tracking-widest mt-1 opacity-80">
              {{ label }}
            </span>
          </div>
        </div>

        <p v-if="!isFinished" class="text-xs font-light italic opacity-70">
          (Rafraîchis la page pour + de fun)
        </p>

        <div v-else class="flex flex-col items-center gap-4 animate-pulse">
          <p class="text-3xl md:text-5xl font-bold drop-shadow-lg">🎉 C'est l'heure !</p>
          <p class="text-lg md:text-2xl font-light">Rends-toi ici :</p>
          <p class="text-2xl md:text-4xl font-bold whitespace-pre-line drop-shadow-lg">
            {{ SURPRISE_ADDRESS }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>
