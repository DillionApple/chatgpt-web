<script lang="ts" setup>
import { ref } from 'vue'
import { NButton, NInput, NSlider, NSelect, useMessage } from 'naive-ui'
import { useSettingStore } from '@/store'
import type { SettingsState } from '@/store/modules/settings/helper'
import { t } from '@/locales'

const settingStore = useSettingStore()

const ms = useMessage()

const systemMessage = ref(settingStore.systemMessage ?? '')

const temperature = ref(settingStore.temperature ?? 0.5)

const top_p = ref(settingStore.top_p ?? 1)

const req_model = ref(settingStore.req_model ?? 'azure-gpt-4-0125-preview')

const options = [
{
  label: 'deepseek-chat (DeepSeek-V2)',
  value: 'deepseek-chat',
}, {
  label: 'deepseek-coder',
  value: 'deepseek-coder',
}, {
  label: 'gpt-4',
  value: 'azure-gpt-4',
}, {
  label: 'gpt-4o',
  value: 'gpt-4o-2024-05-13-high-five-ai',
}, {
  label: 'gpt-4-turbo (2024-04-09)',
  value: 'gpt-4-turbo-2024-04-09-high-five-ai',
}, {
  label: 'gpt-3.5-turbo (2024-01-25)',
  value: 'azure-gpt-35-turbo-0125',
}, {
  label: 'claude-3-sonnet',
  value: 'claude-3-sonnet-20240229',
}, {
  label: 'claude-3-opus',
  value: 'claude-3-opus-20240229',
}]

function updateSettings(options: Partial<SettingsState>) {
  settingStore.updateSetting(options)
  ms.success(t('common.success'))
}

function handleReset() {
  settingStore.resetSetting()
  ms.success(t('common.success'))
  window.location.reload()
}
</script>

<template>
  <div class="p-4 space-y-5 min-h-[200px]">
    <div class="space-y-6">
      <div class="flex items-center space-x-4">
        <span class="flex-shrink-0 w-[120px]">{{ $t('setting.role') }}</span>
        <div class="flex-1">
          <NInput v-model:value="systemMessage" type="textarea" :autosize="{ minRows: 1, maxRows: 4 }" />
        </div>
        <NButton size="tiny" text type="primary" @click="updateSettings({ systemMessage })">
          {{ $t('common.save') }}
        </NButton>
      </div>
      <div class="flex items-center space-x-4">
        <span class="flex-shrink-0 w-[120px]">{{ $t('setting.temperature') }} </span>
        <div class="flex-1">
          <NSlider v-model:value="temperature" :max="2" :min="0" :step="0.1" />
        </div>
        <span>{{ temperature }}</span>
        <NButton size="tiny" text type="primary" @click="updateSettings({ temperature })">
          {{ $t('common.save') }}
        </NButton>
      </div>
      <div class="flex items-center space-x-4">
        <span class="flex-shrink-0 w-[120px]">{{ $t('setting.top_p') }} </span>
        <div class="flex-1">
          <NSlider v-model:value="top_p" :max="1" :min="0" :step="0.1" />
        </div>
        <span>{{ top_p }}</span>
        <NButton size="tiny" text type="primary" @click="updateSettings({ top_p })">
          {{ $t('common.save') }}
        </NButton>
      </div>
      <div class="flex items-center space-x-4">
        <span class="flex-shrink-0 w-[120px]">Model </span>
        <div class="flex-1">
          <NSelect
            v-model:value="req_model"
            :options="options"
            @update:value="updateSettings({ req_model })"
          />
        </div>
      </div>
      <div class="flex items-center space-x-4">
        <span class="flex-shrink-0 w-[120px]">&nbsp;</span>
        <NButton size="small" @click="handleReset">
          {{ $t('common.reset') }}
        </NButton>
      </div>
    </div>
  </div>
</template>
