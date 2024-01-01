<script setup lang="ts">
import { NInput, NButton, useMessage} from "naive-ui"
import { gptServerStore } from '@/store'
import { mlog, myTrim } from "@/api";
import { t } from '@/locales'

const emit= defineEmits(['close']);
const ms= useMessage();

// 新增：固定地址
const fixedOpenAIUrl = 'https://api.132999.xyz';
const fixedUploaderUrl = 'https://tu.132999.xyz/upload';

const save = ()=>{
    // 更新：使用固定地址
    gptServerStore.myData.OPENAI_API_BASE_URL = fixedOpenAIUrl;
    gptServerStore.myData.UPLOADER_URL = fixedUploaderUrl;
    gptServerStore.setMyData(gptServerStore.myData);
    ms.success(t('mjchat.success'));
    emit('close');
}

const blurClean= ()=>{
  mlog('blurClean');
  // 更新：移除对 OPENAI_API_BASE_URL 和 UPLOADER_URL 的处理
  gptServerStore.myData.OPENAI_API_KEY = gptServerStore.myData.OPENAI_API_KEY.trim();
  gptServerStore.myData.MJ_SERVER = myTrim(myTrim(gptServerStore.myData.MJ_SERVER.trim(),'/'), '\\');
  gptServerStore.myData.MJ_API_SECRET = gptServerStore.myData.MJ_API_SECRET.trim();
}
</script>

<template>
<div id="setserver"> 
<div class="text-right">{{ $t('mj.setOpen') }}</div>
<section class="mb-4 flex justify-between items-center"  >
    <!-- 更新：固定 OpenAI 接口地址并禁用 -->
    <n-input :value="fixedOpenAIUrl" disabled :placeholder="$t('mj.setOpenPlaceholder')">
      <template #prefix>
        <span class="text-[var(--n-tab-text-color-active)]">{{ $t('mj.setOpenUrl') }}:</span>
      </template>
    </n-input>
 </section>

 <!-- 现有的 OpenAI Key 和 Midjourney 相关部分保持不变 -->

 <div  class="text-right" > {{$t('mj.setUploader')}}</div>
<section class="mb-4 flex justify-between items-center"  >
    <!-- 更新：固定上传地址并禁用 -->
    <n-input :value="fixedUploaderUrl" disabled :placeholder="$t('mj.setOpenPlaceholder')">
      <template #prefix>
        <span class="text-[var(--n-tab-text-color-active)]">{{$t('mj.setUploaderUrl')}}</span>
      </template>
    </n-input>
 </section>

 <!-- 保存和恢复默认按钮保持不变 -->
</div>
</template>

<style>
#setserver .n-input .n-input__input-el{
    text-align: right;
}
</style>
