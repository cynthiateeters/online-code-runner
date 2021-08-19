<template>
  <div class="app">
    <h2>Online code runner</h2>
    <div class="monaco">
      <Monaco ref="monaco" />
    </div>
    <div class="buttons">
      <button @click="runCode">Click to run code</button>
      <button class="clear" @click="clearConsole">Clear</button>
    </div>
    <div class="console">
      <Console ref="console" />
    </div>
  </div>
  <iframe
    style="display: none;"
    ref="iframe"
    :src="pathname"
    sandbox="allow-same-origin allow-scripts">
  </iframe>
</template>

<script>
/* eslint-disable no-useless-escape */
import Monaco from './components/Monaco.vue';
import Console from './components/Console.vue';
import codeTpl from './utils/codeTemplate';

export default {
  name: 'App',
  components: {
    Monaco,
    Console,
  },
  data() {
    return {
      pathname: `${location.pathname}preview.html`,
    };
  },
  methods: {
    runCode() {
      const code = this.$refs.monaco.submit();
      const { iframe } = this.$refs;
      const iframeDoc = iframe.contentDocument;
      iframeDoc.open();
      iframeDoc.write(codeTpl(code));
      iframeDoc.close();
    },
    clearConsole() {
      this.$refs.console.clear();
    }
  },
};
</script>

<style lang="less">
.app {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 10px;
  box-sizing: border-box;
  h2 {
    margin: 15px 0;
    font-family: fantasy;
  }
  .monaco,
  .console {
    width: 100%;
    border: 1px solid hsl(0, 0%, 30%);
    box-sizing: border-box;
    border-radius: 14px;
    overflow: hidden;
  }
  .buttons {
    width: 100%;
    display: flex;
    button {
      margin: 5px 0;
      border: none;
      width: max-content;
      border: 2px solid hsl(135, 72%, 43%);
      background: hsl(135, 72%, 55%);
      height: 5vh;
      color: #fff;
      border-radius: 4px;
      &.clear {
        width: max-content;
        border: 2px solid hsl(0, 1%, 35%);
        background: hsl(0, 1%,45%);
        margin-left: 5px;
      }
    }
  }
}
</style>
