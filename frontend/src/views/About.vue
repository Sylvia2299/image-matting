<template>
  <div class="p-6 max-full flex flex-col items-center">
    <h2 class="text-2xl font-bold">{{ t('about.title') }}</h2>
    <div class="w-full max-w-lg p-6 rounded-lg shadow-md select-text">
      <h3 class="text-xl font-semibold">{{ t('about.system_info') }}</h3>
      <p class=" mb-2">{{ t('about.author') }}: {{ systemInfo.author }}</p>
      <p class=" mb-2">{{ t('about.email') }}: {{ systemInfo.email }}</p>
      <p class="mb-2">Github:<span class="cursor-pointer text-blue-500" @click="openLink(systemInfo.github)">{{
        systemInfo.github }}</span></p>
      <p class="mb-2">Website: <span class="cursor-pointer text-blue-500" @click="openLink(systemInfo.website)">{{
        systemInfo.website }}</span></p>
      <p class=" mb-2">{{ t('about.version') }}: {{ systemInfo.version }}</p>
      <p class=" mb-2">{{ t('about.sponsor') }}:</p>
      <div>
        <img src="/wx_sponsor.png" alt="sponsor QR Code" class="h-36 object-cover rounded-md" />
      </div>
      <p class=" mb-2">{{ t('about.wx_info') }}:</p>
      <div class="flex justify-center">
        <img src="/wx_qr.png" alt="WeChat QR Code" class="h-32 w-full rounded-md" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import { useI18n } from 'vue-i18n'
import baseAPI from '@/api/base';


const { locale } = useI18n();
const { t } = useI18n()
const systemInfo = ref({
  'author': '',
  'email': '',
  'version': '',
  'github': '',
  'website': '',
})


const openLink = async (url) => {
  await baseAPI('open_link', url)
}


// 获取系统信息
const getSystemInfo = async () => {
  const res = await baseAPI('get_system_info')
  if (res.code === 200) {
    systemInfo.value = res.data
  }
}

onMounted(async () => {
  await getSystemInfo();
})

</script>