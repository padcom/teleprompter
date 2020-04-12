<template>
  <div>
    <p>
      <select v-model="url">
        <option v-for="guide in guides" :key="guide" :value="guide">{{ guide }}</option>
      </select>
    </p>
    <button @click="prev" :disabled="isFirstStep">&lt;&lt;&lt; Previous</button>
    <button @click="loadStepDatatoClipboard">Copy</button>
    <button @click="next" :disabled="isLastStep">Next &gt;&gt;&gt;</button>
    <hr />
    <div v-if="current">
      <h1>{{ current.title }}</h1>
      <p v-for="(line, index) in current.description" :key="index" class="description">{{ line }}</p>
      <textarea :value="current.content" rows="30" cols="100"></textarea>
    </div>
  </div>
</template>

<script>
import { copyToClipboard } from './utils'

export default {
  data() {
    return {
      step: 0,
      steps: [],
      url: '',
      guides: [
        './pure-html.txt', './vue-cli.txt', '/parcel.txt', 
      ]
    }
  },
  computed: {
    current() {
      if (this.steps.length === 0) return null
      else return this.steps[this.step]
    },
    isLastStep() {
      return this.current === null || this.step === this.steps.length - 1
    },
    isFirstStep() {
      return this.current === null || this.step === 0
    }
  },
  watch: {
    url(newUrl) {
      this.loadSteps(newUrl)
    }
  },
  mounted () {
    window.addEventListener('keypress', this.handleKeyboard)
    this.url = this.guides[0]
    this.loadSteps(this.url)
  },
  beforeDestroy () {
    window.removeEventListener('keypress', this.handleKeyboard)
  },
  methods: {
    async loadSteps(url) {
      this.steps = []
      const HEADER_RX = new RegExp(/^\d+\. /)
      const response = await fetch(url)
      const text = await response.text()
      const lines = text.split('\n')
      let row = 0
      let step
      while (row < lines.length) {
        if (HEADER_RX.test(lines[row])) {
          step = { title: lines[row], content: '', description: [] }
          this.steps.push(step)
        } else if (lines[row].startsWith('> ')) {
          if (step.content) {
            step.content += '\r'
          }
          step.content += lines[row].substring(2)
        } else if (step) {
          step.description.push(lines[row])
        }
        row++
      }
      for (let i = 0; i < this.steps.length; i++) {
        this.steps[i].content.trim()
      }
      this.step = 0
      this.loadStepDatatoClipboard()
    },
    handleKeyboard(e) {
      if (e.code === 'KeyZ') this.prev()
      else if (e.code === 'KeyX') this.next()
    },
    next () {
      if (this.step < this.steps.length - 1) {
        this.step++
        this.loadStepDatatoClipboard()
      }
    },
    prev () {
      if (this.step > 0) {
        this.step--
        this.loadStepDatatoClipboard()
      }
    },
    async loadStepDatatoClipboard() {
      await this.$nextTick()
      // console.log('this.steps', this.steps)
      // console.log('this.step', this.step)
      // console.log('this.steps[this.step]', this.steps[this.step])
      copyToClipboard(this.steps[this.step].content)
    }
  }
}
</script>

<style>
.description {
  font-family: sans;
  font-size: 26px;
  font-weight: bold;
}
</style>
