<template>
  <el-form label-width="auto"
           style="width: 100%"
           @submit="(event)=>{
                    event.preventDefault()
                   }">
    <el-form-item label="Mikan">
      <el-input v-model:model-value="props.config.mikanHost" placeholder="https://mikanime.tv"/>
    </el-form-item>
    <el-form-item label="TmdbApi">
      <el-input v-model:model-value="props.config['tmdbApi']" placeholder="https://api.themoviedb.org"/>
    </el-form-item>
    <el-form-item label="TmdbApiKey">
      <el-input v-model:model-value="props.config['tmdbApiKey']" placeholder="请自备 API 密钥, 留空使用系统默认"/>
    </el-form-item>
    <el-form-item label="BgmToken">
      <div style="width: 100%;">
        <el-input v-model:model-value="props.config.bgmToken" show-password type="password"/>
        <div>
          <el-text class="mx-1" size="small">
            你可以在 <a href="https://next.bgm.tv/demo/access-token"
                        target="_blank">https://next.bgm.tv/demo/access-token</a>
            生成一个 Access Token
            <br>
            <a href="http://docs.wushuo.top/config/basic/other#emby-webhook" target="_blank">支持自动点格子</a>
          </el-text>
        </div>
      </div>
    </el-form-item>
    <el-form-item label="最大日志条数">
      <div style="width: 150px">
        <el-select v-model:model-value="props.config.logsMax">
          <el-option v-for="it in [1024,2048,4096,8192]" :key="it" :label="it" :value="it"/>
        </el-select>
      </div>
    </el-form-item>
    <el-form-item label="自动更新">
      <div style="width: 100%;">
        <div>
          <el-switch v-model:model-value="props.config.autoUpdate"/>
        </div>
        <div>
          <el-text class="mx-1" size="small">
            每天 06:00 自动更新程序
          </el-text>
        </div>
      </div>
    </el-form-item>
    <el-form-item label="GitHub加速">
      <div>
        <div>
          <el-select v-model="props.config['github']" style="width: 150px;">
            <el-option v-for="it in githubList" :key="it" :label="it" :value="it"/>
          </el-select>
        </div>
        <div>
          <el-checkbox v-model="props.config['customGithub']" label="自定义"/>
        </div>
        <div v-if="props.config['customGithub']">
          <el-input v-model="props.config['customGithubUrl']" placeholder="https://xxxx.com"/>
        </div>
      </div>
    </el-form-item>
    <el-form-item label="DEBUG">
      <el-switch v-model:model-value="props.config.debug"/>
    </el-form-item>
    <el-form-item label="缓存">
      <div style="width: 100%;">
        <div>
          <el-button :loading="clearCacheLoading" bg icon="Delete" text @click="clearCache">清理</el-button>
        </div>
        <div>
          <el-text class="mx-1" size="small">
            清理现在不被使用的缓存
          </el-text>
        </div>
      </div>
    </el-form-item>
    <el-form-item label="自动GC间隔">
      <el-input-number v-model:model-value="props.config['gcSleep']" :min="0">
        <template #suffix>
          <span>分钟</span>
        </template>
      </el-input-number>
    </el-form-item>
    <el-form-item label="自动备份配置">
      <div>
        <el-switch v-model="props.config['configBackup']"/>
        <br>
        <el-input-number v-model="props.config['configBackupDay']" :min="1">
          <template #suffix>
            <span>天</span>
          </template>
        </el-input-number>
      </div>
    </el-form-item>
  </el-form>
</template>

<script setup>
import {ElMessage, ElText} from "element-plus";
import {ref} from "vue";
import api from "@/js/api.js";

let githubList = new Set([
  'None',
  'gh-proxy.com',
  'gh.h233.eu.org',
  'ghproxy.1888866.xyz',
  'slink.ltd',
  'hub.gitmirror.com',
  'github.boki.moe',
  'github.moeyy.xyz',
  'gh-proxy.net',
  'ghfast.top',
  'pd.zwc365.com',
  'ghproxy.cfd',
  'gh.jasonzeng.dev',
  'gh.monlor.com',
  'github.tbedu.top',
  'gh-proxy.linioi.com',
  'mirrors.chenby.cn',
  'github.ednovas.xyz',
  'ghp.keleyaa.com',
  'github.wuzhij.com',
  'gh.cache.cloudns.org',
  'gh.chjina.com',
  'ghpxy.hwinzniej.top',
  'cdn.crashmc.com',
  'gitproxy.mrhjx.cn',
  'gh.xxooo.cf',
  'gh.944446.xyz',
  'api-gh.muran.eu.org',
  'gh.zwnes.xyz',
  'gh.llkk.cc',
  'gh-proxy.ygxz.in',
  'gh.nxnow.top',
  'gh-proxy.ygxz.in',
  'gh.zwy.one',
  'ghproxy.monkeyray.net',
  'gh.xx9527.cn',
  'ghfast.top'
])

let clearCacheLoading = ref(false)
let clearCache = () => {
  clearCacheLoading.value = true
  api.post('api/clearCache')
      .then(res => {
        ElMessage.success(res.message);
      })
      .finally(() => {
        clearCacheLoading.value = false
      })
}

let props = defineProps(['config'])
</script>