<script setup>
import { ref } from 'vue'

const input = ref('')
const messages = ref([])
const isTyping = ref(false)

const examples = [
  {
    prompt: '/plan 添加用户登录功能',
    response: '## 实施计划\n\n1. 创建 auth 模块\n2. 实现 JWT token 生成\n3. 添加登录/注册 API\n4. 编写单元测试\n5. 集成测试验证\n\n**预估复杂度**: Medium',
  },
  {
    prompt: '修复这个 TypeScript 类型错误',
    response: '找到问题：`user.name` 可能为 `undefined`。\n\n修复：添加可选链 `user?.name ?? "Guest"`\n\n已更新 `src/utils/user.ts:42`',
  },
  {
    prompt: '给这个函数写测试',
    response: '已生成 3 个测试用例：\n\n- ✅ 正常输入返回预期结果\n- ✅ 边界值处理正确\n- ✅ 异常输入抛出错误\n\n覆盖率: 94%',
  },
]

async function sendMessage() {
  if (!input.value.trim()) return

  const userMsg = input.value.trim()
  messages.value.push({ role: 'user', content: userMsg })
  input.value = ''
  isTyping.value = true

  const match = examples.find((e) =>
    userMsg.includes(e.prompt.slice(0, 6))
  )
  const response = match?.response ?? '已完成任务。共修改 3 个文件，添加 47 行代码。'

  await new Promise((r) => setTimeout(r, 1200))

  isTyping.value = false
  messages.value.push({ role: 'assistant', content: response })
}

function useExample(idx) {
  input.value = examples[idx].prompt
  sendMessage()
}
</script>

<template>
  <div class="prompt-demo">
    <div class="terminal">
      <div class="terminal-header">
        <span class="dot red" />
        <span class="dot yellow" />
        <span class="dot green" />
        <span class="ml-2 opacity-50 text-xs">claude</span>
      </div>
      <div class="terminal-body">
        <div v-if="messages.length === 0" class="text-center opacity-40 py-4">
          <p>试试点击下面的示例 / Try an example below</p>
        </div>
        <div
          v-for="(msg, i) in messages"
          :key="i"
          class="message"
          :class="msg.role"
        >
          <span class="prefix">{{ msg.role === 'user' ? '>' : '◆' }}</span>
          <span class="whitespace-pre-wrap">{{ msg.content }}</span>
        </div>
        <div v-if="isTyping" class="message assistant">
          <span class="prefix">◆</span>
          <span class="typing">思考中...</span>
        </div>
      </div>
      <div class="terminal-input">
        <span class="opacity-50">></span>
        <input
          v-model="input"
          placeholder="输入提示词 / Type a prompt..."
          @keydown.enter="sendMessage"
        />
      </div>
    </div>
    <div class="examples">
      <button
        v-for="(ex, i) in examples"
        :key="i"
        class="example-btn"
        @click="useExample(i)"
      >
        {{ ex.prompt }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.prompt-demo {
  width: 100%;
  max-width: 640px;
  margin: 0 auto;
}

.terminal {
  background: #1a1a2e;
  border-radius: 8px;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.terminal-header {
  display: flex;
  align-items: center;
  padding: 8px 12px;
  background: rgba(255, 255, 255, 0.05);
}

.dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  margin-right: 6px;
}

.dot.red { background: #ff5f57; }
.dot.yellow { background: #febc2e; }
.dot.green { background: #28c840; }

.terminal-body {
  padding: 12px;
  max-height: 240px;
  overflow-y: auto;
  font-size: 13px;
  font-family: 'Fira Code', monospace;
}

.message {
  margin-bottom: 8px;
  line-height: 1.5;
}

.message.user { color: #7ec8e3; }
.message.assistant { color: #c8e6c9; }

.prefix {
  margin-right: 8px;
  opacity: 0.6;
}

.typing {
  animation: blink 1s infinite;
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.3; }
}

.terminal-input {
  display: flex;
  align-items: center;
  padding: 8px 12px;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  font-family: 'Fira Code', monospace;
  font-size: 13px;
}

.terminal-input input {
  flex: 1;
  background: none;
  border: none;
  color: white;
  outline: none;
  margin-left: 8px;
  font-family: inherit;
  font-size: inherit;
}

.examples {
  display: flex;
  gap: 8px;
  margin-top: 12px;
  flex-wrap: wrap;
}

.example-btn {
  padding: 4px 12px;
  border-radius: 16px;
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.15);
  color: rgba(255, 255, 255, 0.7);
  font-size: 12px;
  cursor: pointer;
  transition: all 0.2s;
}

.example-btn:hover {
  background: rgba(255, 255, 255, 0.15);
  color: white;
}
</style>
