<script setup lang="ts">

const openModal = ref(false)
const ratingValue = ref(0)
const descriptionValue = ref("")

const handleRatingSelected = (value: number) => {
  switch (value) {
    case 0.5:
    case 1:
      descriptionValue.value = "Aïe, pas top"
      return
    case 1.5:
    case 2:
      descriptionValue.value = "Moyennasse"
      return
    case 2.5:
    case 3:
      descriptionValue.value = "Bien"
      return
    case 3.5:
    case 4:
      descriptionValue.value = "Lourd !"
      return
    case 4.5:
    case 5:
      descriptionValue.value = "Incroyable !"
      return
    default:
      descriptionValue.value = ""
      return
  }
}

watch(openModal, () => {
  if (!openModal.value) {
    messageValidation.value = ''
    ratingValue.value = 0
    descriptionValue.value = ""
  }
})

const messageValidation = ref('')
const loadingSubmission = ref(false)
const alreadySubmitted = ref(false)

const handleSubmission = () => {
  loadingSubmission.value = true

  setTimeout(() => {
    messageValidation.value = 'Merci pour ta note !'
    loadingSubmission.value = false
  }, 2000)
  alreadySubmitted.value = true
}

const handleStarClick = (event: MouseEvent) => {
  const target = event.target as HTMLElement;
  const starElement = target.closest('.nuxt-rating-star');

  if (starElement) {
    const wrapper = starElement.closest('.nuxt-rating-wrapper');
    if (wrapper) {
      const previouslyClicked = wrapper.querySelector('.nuxt-rating-star.clicked');
      if (previouslyClicked && previouslyClicked !== starElement) {
        previouslyClicked.classList.remove('clicked');
      }
    }
    starElement.classList.add('clicked');
  }
}

</script>

<template>
  <div class="flex flex-col items-center justify-center gap-4 h-screen">

    <div class="flex flex-col items-center gap-4">
      <h1 class="text-2xl font-bold">Bienvenue dans mon challenge</h1>
      <UModal 
        v-model:open="openModal" 
        :ui="{ footer: 'justify-end', body: 'p-4 flex flex-col items-center gap-4' , overlay: 'bg-[#7B61FF]'}">
      <UButton label="Donner une note au challenge" color="neutral" variant="subtle" />

    <template #body>
      <h2 class="text-2xl font-semibold text-center">
        Quel note donnerais-tu à ce challenge ?
      </h2>
      <p class="text-center text-sm">
        Bon j’espère que tu vas mettre 5 évidemment, si ce n’est pas le cas viens me dire pourquoi !
      </p>
      <NuxtRating
        v-model:rating-value="ratingValue"
        class="flex gap-4"
        inactive-color="white"
        :read-only="false" 
        active-color="#7B61FF"
        border-color="#7B61FF"
        :border-width="4"
        :rating-step="0.5"
        skeleton-color="#7B61FF"
        rating-size="26"
        @rating-selected="(value) => handleRatingSelected(value)"
        @click="handleStarClick"
      />
      <p class="text-center my-2">{{ descriptionValue }}</p>
    </template>

    <template #footer>
      <UButton v-if="!loadingSubmission && ! alreadySubmitted" label="Soumettre ma note" color="neutral" :disabled="descriptionValue === ''" @click="handleSubmission"/>
      <p v-if="loadingSubmission">Enregistrement de ta note...</p>
      <p v-else>{{ messageValidation }}</p>
    </template>
  </UModal>

    </div>
  </div>
</template>

<style>
.clicked {
  animation: clicked 0.4s ease-in-out;
}

@keyframes clicked {
  0% {
    background: #7B61FF20;
    border-radius: 50px;
  }
  100% {
    background: transparent;
  }
}
</style>