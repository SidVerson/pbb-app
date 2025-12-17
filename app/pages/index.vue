<!-- pages/index.vue -->
<template>
  <UContainer class="min-h-screen flex items-center justify-center">
    <UCard class="w-full max-w-md">
      <div class="text-center">
        <h1 class="text-2xl font-bold mb-4">
          Давайте познакомимся поближе!
        </h1>
        <div class="space-y-4">
          <UButton
            block
            size="lg"
            color="primary"
            @click="navigateTo('/quiz/mom')"
          >
            Я… Беременная или уже мама
          </UButton>

          <UButton
            block
            size="lg"
            variant="outline"
            @click="navigateTo('/quiz/donor')"
          >
            Супруг/родственник/друг
          </UButton>
        </div>
      </div>
    </UCard>
  </UContainer>
</template>

<script setup>
const cancel = () => {
  // Логика отмены
  navigateTo('/')
}
</script>
