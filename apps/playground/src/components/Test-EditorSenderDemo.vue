<!-- 富文本发送框 测试中... -->
<script setup lang="ts">
// import type { TriggerEvent } from 'vue-element-plus-x/src/components/Sender/types.d.ts'
import { CircleClose, Link } from '@element-plus/icons-vue'
import EditorSender from 'vue-element-plus-x/src/components/EditorSender/index.vue'

const senderRef = ref()
const value = ref('')
const loading = ref(false)
const showHeaderFlog = ref(false)
const time = ref<null | number>(null)

function openCloseHeader() {
  if (!showHeaderFlog.value) {
    senderRef.value.openHeader()
  }
  else {
    senderRef.value.closeHeader()
  }
  showHeaderFlog.value = !showHeaderFlog.value
}

function closeHeader() {
  showHeaderFlog.value = false
  senderRef.value.closeHeader()
}

function submit() {
  console.log('submit 提交参数', value.value)
  ElMessage.warning('提交中,💌 注意组件内置提交按钮的变化 ~')
  loading.value = true
  time.value = setTimeout(() => {
    loading.value = false
    ElMessage.success('提交成功')
  }, 3500)
}

// 取消提交
function cancel() {
  ElMessage.info('取消提交')
  loading.value = false
  if (time.value)
    clearTimeout(time.value)
  time.value = null
}

function blur() {
  senderRef.value.blur()
}

function focus(type = 'all') {
  senderRef.value.focus(type)
}

const triggerPopoverVisible = ref(false)
</script>

<template>
  <div class="component-container">
    <div class="top-wrap">
      <div class="btn-list">
        <el-button dark type="success" plain @click="focus('start')">
          文本最前方
        </el-button>
        <el-button dark type="success" plain @click="focus('end')">
          文本最后方
        </el-button>
        <el-button dark type="success" plain @click="focus('all')">
          整个文本
        </el-button>
        <el-button dark type="success" plain @click="blur">
          取消焦点
        </el-button>
        <el-button dark type="success" plain @click="senderRef.openHeader()">
          打开头部
        </el-button>
        <el-button dark type="success" plain @click="senderRef.closeHeader()">
          关闭头部
        </el-button>
        <el-button dark type="success" plain @click="senderRef.submit()">
          提交
        </el-button>
        <el-button dark type="success" plain @click="cancel">
          取消提交
        </el-button>

        <el-button dark type="success" plain @click="senderRef.startRecognition()">
          开始录音
        </el-button>
        <el-button dark type="success" plain @click="senderRef.stopRecognition()">
          结束录音
        </el-button>
      </div>
    </div>

    <div class="component-1">
      <EditorSender
        ref="senderRef"
        v-model="value"
        v-model:trigger-popover-visible="triggerPopoverVisible"
        allow-speech
        clearable
        submit-type="shiftEnter"
        placeholder="使用 / 或者 @ 触发指令，可支持自定义多个指令"
        :loading="loading"
        :trigger-strings="['/', '@']"
        variant="updown"
        @submit="submit"
      >
        <!-- 自定义头部 -->
        <template #header>
          <div class="header-self-wrap">
            <div class="header-self-title">
              <div class="header-left">
                自定义头部标题
              </div>
              <div class="header-right">
                <el-button @click.stop="closeHeader">
                  <el-icon><CircleClose /></el-icon>
                  <span>关闭头部</span>
                </el-button>
              </div>
            </div>
            <div class="header-self-content">
              自定义头部内容
            </div>
          </div>
        </template>

        <!-- 自定义前缀 -->
        <template #prefix>
          <div class="prefix-self-wrap">
            <el-button dark @click="openCloseHeader">
              <el-icon><Link /></el-icon>
              <span>自定义前缀</span>
            </el-button>
          </div>
        </template>

        <!-- 自定义 指令触发的 popover -->
        <!-- <template #trigger-popover="{ triggerString }">
          {{ triggerString }}
        </template> -->

        <!-- 自定义操作列表 -->
        <!-- <template #action-list>
          <div class="action-list-self-wrap">
            <span>自定义操作列表</span>
            <el-button dark type="success" plain @click="focus('start')">
              文本最前方
            </el-button>
            <el-button dark type="success" plain @click="focus('end')">
              文本最后方
            </el-button>
            <el-button dark type="success" plain @click="focus('all')">
              整个文本
            </el-button>
            <el-button dark type="success" plain @click="blur">
              取消焦点
            </el-button>
          </div>
        </template> -->

        <!-- 自定义底部 -->
        <template #footer>
          <div class="footer-self-wrap">
            <div class="prefix-self-wrap">
              <el-button dark @click="openCloseHeader">
                <el-icon><Link /></el-icon>
                <span>自定义前缀</span>
              </el-button>
            </div>
          </div>
        </template>
      </EditorSender>
    </div>
  </div>
</template>

<style scoped lang="scss">
.component-container {
  background-color: white;
  padding: 12px;
  border-radius: 15px;
  min-height: 400px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: calc(100vh - 220px);
  overflow: auto;

  .top-wrap {
    margin: 12px 0;
    .btn-list {
      display: flex;
      flex-wrap: wrap;
      :deep() {
        .el-button + .el-button {
          margin-left: 8px;
          margin-bottom: 8px;
        }
      }
    }
  }

  .component-1 {
    .header-self-wrap {
      flex: 1;
      display: block;
      display: flex;
      flex-direction: column;
      height: 200px;
      margin: 0;
      .header-self-title {
        padding: 16px;
        display: flex;
        height: 30px;
        align-items: center;
        justify-content: space-between;
        padding-bottom: 8px;
      }
      .header-self-content {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 20px;
        color: #626aef;
        font-weight: 600;
      }
    }

    .prefix-self-wrap {
      display: flex;
      flex-wrap: wrap;
    }

    .action-list-self-wrap {
      display: flex;
      align-items: center;
      & > span {
        width: 120px;
        font-weight: 600;
        color: var(--el-color-primary);
      }
    }

    .footer-self-wrap {
      display: flex;
      align-items: center;
    }
  }
}
</style>
