<template>
  <UContainer class="py-10 max-w-3xl">
    <UCard class="shadow-lg">
      <template #header>
        <div class="text-center">
          <h1 class="text-2xl font-bold text-pink-600">
            Здравствуйте, дорогая!
          </h1>
          <p class="text-gray-600 mt-2 max-w-md mx-auto">
            Мы в PBB верим, что каждая беременность уникальна...
          </p>
        </div>
      </template>

      <UForm
        :state="form"
        class="space-y-8"
        @submit="submitForm"
      >
        <!-- 1. Текущий срок -->
        <UFormField
          label="1. Подскажите ваш текущий срок?"
          required
          name="trimester"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <URadioGroup
            v-model="form.trimester"
            :items="trimesterOptions"
            :ui="{ wrapper: 'grid grid-cols-1 sm:grid-cols-2 gap-2' }"
          />
        </UFormField>

        <!-- 2. Дата ПДР -->
        <UFormField
          label="2. Поделитесь с нами волшебной датой — вашей ПДР?"
          required
          name="dueDate"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <UInput
            v-model="form.dueDate"
            type="date"
            :ui="{ base: 'w-full' }"
          />
        </UFormField>

        <!-- 3. Первая беременность -->
        <UFormField
          label="3. Это ваша первая беременность?"
          required
          name="isFirstPregnancy"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <URadioGroup
            v-model="form.isFirstPregnancy"
            :items="yesNoOptions"
            :ui="{ wrapper: 'flex gap-4' }"
          />
        </UFormField>

        <!-- 4. Тип кожи -->
        <UFormField
          label="4. Как обычно ведет себя ваша кожа?"
          required
          name="skinType"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <USelect
            v-model="form.skinType"
            :items="skinTypeOptions"
            placeholder="Выберите вариант"
            class="w-full"
          />
        </UFormField>

        <!-- 5. Просьбы от тела -->
        <UFormField
          label="5. Есть ли особые «просьбы» от вашего тела в это время?"
          required
          name="bodyNeeds"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <UCheckboxGroup
            v-model="form.bodyNeeds"
            :items="bodyNeedsOptions"
            :ui="{ wrapper: 'grid grid-cols-1 md:grid-cols-2 gap-3' }"
          >
          </UCheckboxGroup>
        </UFormField>

        <!-- 6. Предпочтения по ароматам -->
        <UFormField
          label="6. А что насчет ароматов?"
          required
          name="aromaPreference"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <URadioGroup
            v-model="form.aromaPreference"
            :items="aromaOptions"
            :ui="{ wrapper: 'grid grid-cols-1 md:grid-cols-2 gap-2' }"
          />
        </UFormField>

        <!-- 7. Аллергии -->
        <UFormField
          label="7. Есть ли у вас аллергия или компоненты, которых стоит избегать?"
          name="allergies"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <UTextarea
            v-model="form.allergies"
            placeholder="например, «эфирное масло апельсина», «ланолин» или «нет, все хорошо»"
            class="w-full"
          />
        </UFormField>

        <!-- 8. Чувствительность к запахам -->
        <UFormField
          label="8. Чувствительность к запахам сейчас — это про вас?"
          required
          name="smellSensitivity"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <URadioGroup
            v-model="form.smellSensitivity"
            :items="smellSensitivityOptions"
            :ui="{ wrapper: 'space-y-2' }"
          />
        </UFormField>

        <!-- 9. Желания от средств -->
        <UFormField
          label="9. Чего бы вам правда хотелось от средств в боксе?"
          required
          name="desires"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <UCheckboxGroup
            v-model="form.desires"
            :items="desireOptions"
            value
            :ui="{ wrapper: 'grid grid-cols-1 md:grid-cols-2 gap-3' }"
          >
          </UCheckboxGroup>
        </UFormField>

        <!-- 10. Настроение -->
        <UFormField
          label="10. Опишите свое настроение в одном предложении или слове"
          name="mood"
          :ui="{ label: { base: 'font-medium' } }"
        >
          <UInput
            class="w-full"
            v-model="form.mood"
            placeholder="например, «В ожидании чуда», «Нуждаюсь в спокойствии», «Полна энергии!»"
          />
        </UFormField>

        <!-- Блок контактов и доставки -->
        <div class="border-t border-gray-200 pt-8">
          <h2 class="text-xl font-semibold mb-6 text-center text-pink-600">
            💝 Контактная информация
          </h2>

          <!-- 11. Выбор бокса -->
          <UFormField
            label="Выберите бокс"
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

          <!-- 12. Адрес доставки -->
          <UFormField
            label="Адрес доставки"
            required
            name="delivery"
            :ui="{ label: { base: 'font-medium' } }"
          >
            <div class="flex flex-col space-y-4 my-4">
              <UInput
                v-model="form.delivery.name"
                placeholder="Имя, фамилия получателя"
                :required="true"
              />
              <UInput
                v-model="form.delivery.phone"
                placeholder="Номер телефона получателя"
                :required="true"
              />
              <UTextarea
                v-model="form.delivery.address"
                placeholder="Город и адрес доставки"
                class="w-full"
                :required="true"
              />
            </div>
          </UFormField>

          <!-- 13. Пожелания к открытке -->
          <UFormField
            label="Пожелания к открытке"
            name="cardWishes"
            :ui="{ label: { base: 'font-medium' } }"
          >
            <UTextarea
              v-model="form.cardWishes"
              placeholder="Что написать в открытке?"
              class="w-full"
            />
          </UFormField>

          <!-- 14. Контакты отправителя -->
          <UFormField
            label="Ваши контактные данные"
            required
            name="sender"
            :ui="{ label: { base: 'font-medium' } }"
            class="my-4"
          >
            <div class="flex flex-col my-4 space-y-4">
              <UInput
                v-model="form.sender.name"
                placeholder="Ваше имя"
                :required="true"
              />
              <UInput
                v-model="form.sender.phone"
                placeholder="Ваш телефон"
                :required="true"
              />
              <UInput
                v-model="form.sender.email"
                type="email"
                placeholder="Ваш e-mail"
                :required="true"
              />
            </div>
          </UFormField>

          <!-- 15. Рассылка -->
          <UFormField
            label="Разрешаете ли вы информировать Вас о новинках и скорой доставке боксов?"
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
        </div>

        <!-- Кнопки -->
        <div class="flex flex-col sm:flex-row gap-4 pt-6 justify-center">
          <UButton
            type="submit"
            color="primary"
            size="lg"
            class="w-full sm:w-auto"
          >
            Отправить анкету
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
import {navigateTo} from "#app";

interface MomForm {
  trimester: string
  dueDate: string
  isFirstPregnancy: boolean | null
  skinType: string
  bodyNeeds: string[]
  aromaPreference: string
  allergies: string
  smellSensitivity: string
  desires: string[]
  mood: string
  boxType: string
  delivery: {
    name: string
    phone: string
    address: string
  }
  cardWishes: string
  sender: {
    name: string
    phone: string
    email: string
  }
  allowNewsletter: boolean | null
}

const form = ref<MomForm>({
  trimester: '',
  dueDate: '',
  isFirstPregnancy: null,
  skinType: '',
  bodyNeeds: [],
  aromaPreference: '',
  allergies: '',
  smellSensitivity: '',
  desires: [],
  mood: '',
  boxType: '',
  delivery: {
    name: '',
    phone: '',
    address: ''
  },
  cardWishes: '',
  sender: {
    name: '',
    phone: '',
    email: ''
  },
  allowNewsletter: null
})

// Опции для селектов
const trimesterOptions = [
  { value: '1', label: '1 триместр (1-13 неделя)' },
  { value: '2', label: '2 триместр (14-27 неделя)' },
  { value: '3', label: '3 триместр (28-40+ неделя)' },
  { value: 'postpartum', label: 'Послеродовой период (до 1 года)' }
]

const skinTypeOptions = [
  'Нормальная, беспроблемная',
  'Часто ощущает сухость и стянутость',
  'Склонна к блеску, особенно в Т-зоне',
  'Капризничает, легко краснеет',
  'Пока не могу определить, все изменилось с беременностью'
]

const bodyNeedsOptions = [
  { value: 'moisturizing', label: 'Больше увлажнения для растущего животика' },
  { value: 'pigmentation', label: 'Помощь с пигментацией или сосудистыми звездочками' },
  { value: 'sensitive', label: 'Уход за чувствительной кожей' },
  { value: 'hair', label: 'Хочется, чтобы волосы блестели, как раньше' },
  { value: 'general', label: 'Пока все хорошо, просто хочу баловать себя' }
]

const aromaOptions = [
  { value: 'floral', label: 'Нежные цветочные (роза, лаванда)' },
  { value: 'fresh', label: 'Свежие, как морской бриз (цитрус, мята)' },
  { value: 'sweet', label: 'Сладкие и уютные (ваниль, карамель)' },
  { value: 'woody', label: 'Спокойные древесные (сандал, кедр)' },
  { value: 'neutral', label: 'Предпочитаю, чтобы средства пахли минимально или нейтрально' }
]

const smellSensitivityOptions = [
  { value: 'high', label: 'Да, к сожалению, даже любимые ароматы могут раздражать' },
  { value: 'medium', label: 'Немного, но терпимо' },
  { value: 'low', label: 'Нет, мой нюх счастлив, как никогда!' }
]

const desireOptions = [
  { value: 'safety', label: 'Максимальной безопасности и натуральности' },
  { value: 'glow', label: 'Мгновенного эффекта «я сияю!»' },
  { value: 'spa', label: 'Возможности создать полноценный спа-вечер дома' },
  { value: 'simplicity', label: 'Простоты — чтобы уход занимал минуты, но радовал' },
  { value: 'trust', label: 'Не знаю, доверяю вашему выбору!' }
]

const boxTypeOptions = [
  { value: 'basic', label: 'Базовый (небольшой приятный сюрприз)' },
  { value: 'comfort', label: 'Комфорт (оптимальный набор для полного ухода)' },
  { value: 'lux', label: 'Lux (премиум-ритуал с эксклюзивными продуктами)' }
]

const yesNoOptions = [
  { value: true, label: 'Да!' },
  { value: false, label: 'Нет' }
]

const submitForm = async () => {
  // Валидация и отправка данных
  console.log('Отправка формы:', form.value)
  navigateTo('/momResult')
  // await $fetch('/api/submit-mom', { method: 'POST', body: form.value })
}

const resetForm = () => {
  form.value = {
    trimester: '',
    dueDate: '',
    isFirstPregnancy: null,
    skinType: '',
    bodyNeeds: [],
    aromaPreference: '',
    allergies: '',
    smellSensitivity: '',
    desires: [],
    mood: '',
    boxType: '',
    delivery: {
      name: '',
      phone: '',
      address: ''
    },
    cardWishes: '',
    sender: {
      name: '',
      phone: '',
      email: ''
    },
    allowNewsletter: null
  }
}
</script>
