<template>
  <UContainer class="py-10 max-w-3xl">
    <UCard class="shadow-lg">
      <template #header>
        <div class="text-center">
          <h1 class="text-2xl font-bold text-pink-600">
            Уважаемый даритель!
          </h1>
          <p class="text-gray-600 mt-2 max-w-md mx-auto">
            Заполните эту короткую форму, и мы гарантированно соберем идеальный бокс...
          </p>
        </div>
      </template>

      <UForm
        :state="form"
        class="space-y-8"
        @submit="submitForm"
      >
        <!-- 1. Период -->
        <UFormField
          label="1. Для какого периода выбираете подарок?"
          required
          name="period"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <URadioGroup
            v-model="form.period"
            :items="periodOptions"
            :ui="{ wrapper: 'grid grid-cols-1 md:grid-cols-2 gap-2' }"
          />
        </UFormField>

        <!-- 2. Тип кожи -->
        <UFormField
          label="2. Примерный тип кожи будущей мамы:"
          required
          name="skinType"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <USelect
            v-model="form.skinType"
            :items="donorSkinTypeOptions"
            placeholder="Выберите тип"
            class="w-full"
          />
        </UFormField>

        <!-- 3. Аллергии -->
        <UFormField
          label="3. Есть ли известные аллергии или непереносимость компонентов?"
          name="allergies"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <UInput
            class="w-full"
            v-model="form.allergies"
            placeholder="Например: &quot;Нет&quot; или &quot;мед, орехи, цитрусовые масла&quot;"
          />
        </UFormField>

        <!-- 4. Отношение к ароматам -->
        <UFormField
          label="4. Отношение к ароматам:"
          required
          name="aromaPreference"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <URadioGroup
            v-model="form.aromaPreference"
            :items="donorAromaOptions"
            :ui="{ wrapper: 'space-y-2' }"
          />
        </UFormField>

        <!-- 5. Категории продуктов -->
        <UFormField
          label="5. Предпочтительная категория продуктов в подарке:"
          required
          name="productCategories"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <UCheckboxGroup
            v-model="form.productCategories"
            :items="productCategories"
            :ui="{ wrapper: 'grid grid-cols-1 md:grid-cols-2 gap-3' }"
          >
            <template #default="{ option }">
              <UCheckbox
                :value="option.value"
                :label="option.label"
              />
            </template>
          </UCheckboxGroup>
        </UFormField>

        <!-- 6. Бюджет -->
        <UFormField
          label="6. Ваш бюджет (выберите бокс):"
          required
          name="boxType"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <URadioGroup
            v-model="form.boxType"
            :items="boxTypeOptions"
            :ui="{ wrapper: 'grid grid-cols-1 md:grid-cols-3 gap-4' }"
          />
        </UFormField>

        <!-- 7. Адрес доставки -->
        <UFormField
          label="7. Адрес доставки и контакт получателя:"
          required
          name="recipient"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <div class="space-y-4 flex flex-col ">
            <UInput
              v-model="form.recipient.name"
              placeholder="Имя, фамилия получателя"
              :required="true"
            />
            <UInput
              v-model="form.recipient.phone"
              placeholder="Номер телефона получателя"
              :required="true"
            />
            <UTextarea
              v-model="form.recipient.address"
              placeholder="Город и адрес доставки"
              class="w-full"
              :required="true"
            />
          </div>
        </UFormField>

        <!-- 8. Пожелания к открытке -->
        <UFormField
          label="8. Ваши пожелания к открытке:"
          name="cardWishes"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <UTextarea
            v-model="form.cardWishes"
            placeholder="Например: &quot;С любовью, от папы и будущего старшего брата&quot;"
            class="w-full"
          />
        </UFormField>

        <!-- 9. Контакты дарителя -->
        <UFormField
          label="9. Контакты дарителя (для связи по заказу):"
          required
          name="donor"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <div class="flex flex-col space-y-4">
            <UInput
              v-model="form.donor.name"
              placeholder="Ваше имя"
              :required="true"
            />
            <UInput
              v-model="form.donor.phone"
              placeholder="Ваш телефон"
              :required="true"
            />
            <UInput
              v-model="form.donor.email"
              type="email"
              placeholder="Ваш e-mail"
              :required="true"
            />
          </div>
        </UFormField>

        <!-- 10. Рассылка -->
        <UFormField
          label="10. Разрешаете ли вы информировать Вас о новинках и скорой доставке боксов?"
          required
          name="allowNewsletter"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <URadioGroup
            v-model="form.allowNewsletter"
            :items="yesNoOptions"
            :ui="{ wrapper: 'flex gap-4' }"
          />
        </UFormField>

        <!-- Кнопки -->
        <div class="flex flex-col sm:flex-row gap-4 pt-6 justify-center">
          <UButton
            type="submit"
            color="primary"
            size="lg"
            class="w-full sm:w-auto"
          >
            Отправить заказ
          </UButton>
          <UButton
            type="button"
            variant="outline"
            color="gray"
            size="lg"
            class="w-full sm:w-auto"
            @click="resetForm"
          >
            Очистить форму
          </UButton>
        </div>
      </UForm>
    </UCard>
  </UContainer>
</template>

<script setup lang="ts">
interface DonorForm {
  period: string
  skinType: string
  allergies: string
  aromaPreference: string
  productCategories: string[]
  boxType: string
  recipient: {
    name: string
    phone: string
    address: string
  }
  cardWishes: string
  donor: {
    name: string
    phone: string
    email: string
  }
  allowNewsletter: boolean | null
}

const form = ref<DonorForm>({
  period: '',
  skinType: '',
  allergies: '',
  aromaPreference: '',
  productCategories: [],
  boxType: '',
  recipient: {
    name: '',
    phone: '',
    address: ''
  },
  cardWishes: '',
  donor: {
    name: '',
    phone: '',
    email: ''
  },
  allowNewsletter: null
})

// Опции
const periodOptions = [
  { value: '1', label: 'Беременность: 1-й триместр (до 13 недель)' },
  { value: '2', label: 'Беременность: 2-й триместр (14-27 недель)' },
  { value: '3', label: 'Беременность: 3-й триместр (28+ недель)' },
  { value: 'postpartum', label: 'Послеродовой период' }
]

const donorSkinTypeOptions = [
  'Нормальная / Комбинированная',
  'Сухая / Чувствительная',
  'Жирная / Склонная к несовершенствам',
  'Не уверены'
]

const donorAromaOptions = [
  { value: 'likes', label: 'Любит цветочные / сладкие / свежие ароматы' },
  { value: 'neutral', label: 'Предпочитает средства без отдушек или с нейтральным запахом' },
  { value: 'unknown', label: 'Не знаю' }
]

const productCategories = [
  { value: 'body', label: 'Уход за телом (масла, лосьоны)' },
  { value: 'face', label: 'Уход за лицом (кремы, маски)' },
  { value: 'relax', label: 'Средства для расслабления (соли для ванн, свечи)' },
  { value: 'tools', label: 'Практичные бьюти-помощники (роллеры, патчи)' },
  { value: 'expert', label: 'Доверяю выбору экспертов PBB' }
]

const boxTypeOptions = [
  { value: 'basic', label: 'Базовый (небольшой приятный сюрприз)' },
  { value: 'comfort', label: 'Комфорт (оптимальный набор для полного ухода)' },
  { value: 'lux', label: 'Lux (премиум-ритуал с эксклюзивными продуктами)' }
]

const yesNoOptions = [
  { value: true, label: 'Да, разрешаю' },
  { value: false, label: 'Нет, не разрешаю' }
]

const submitForm = async () => {
  console.log('Отправка формы дарителя:', form.value)
  navigateTo('/donorResult')
  // await $fetch('/api/submit-donor', { method: 'POST', body: form.value })
}

const resetForm = () => {
  form.value = {
    period: '',
    skinType: '',
    allergies: '',
    aromaPreference: '',
    productCategories: [],
    boxType: '',
    recipient: {
      name: '',
      phone: '',
      address: ''
    },
    cardWishes: '',
    donor: {
      name: '',
      phone: '',
      email: ''
    },
    allowNewsletter: null
  }
}
</script>
