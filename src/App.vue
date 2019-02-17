<template>
  <div id="app">
    <h1 class="title">坂道ロゴジェネレーター</h1>
    <div class="logo">
      <svg xmlns="http://www.w3.org/2000/svg" width="100%" viewBox="0, 0, 150, 150">
        <polygon v-for="(attr, i) in triangleAttributes" :key="`triangle-${i}`" v-bind="attr" />
        <text fill="#fff"  v-bind="mainlineAttrubute" text-anchor="end" font-size="20" font-family="sans-serif">{{mainline}}</text>
        <text y="148" x="128" text-anchor="end" :fill="`#${triangleColors[0]}`" font-family="sans-serif" font-size="16">{{subline}}</text>
      </svg>
    </div>
    <div class="random-shuffle">
      <span @click="initializeParams">シャッフル</span>
    </div>
    <div class="text-field">
      <p>三角の中</p>
      <input v-model="mainline" maxlength="4">
    </div>
    <div class="text-field">
      <p>三角の外</p>
      <input v-model="subline" maxlength="10">
    </div>
    <div class="color-list">
      <div class="color" v-for="(color, i) in triangleColors" :key="`color-${i}`">
        <div class="color-chip">
          <i :style="{borderLeftColor: `#${color}`}"></i>
        </div>
        <div class="color-input">
          <input v-model="triangleColors[i]" maxlength="6">
        </div>
        <div class="color-rm" v-if="isColorRemovable">
          <p @click="removeColorHandler(i)">消す</p>
        </div>
      </div>
      <div class="add-color" @click="addColorHandler">追加</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      subline: '',
      mainline: '',
      triangleColors: [],
    }
  },
  computed: {
    trianglesLen() {
      return this.triangleColors.length;
    },
    mainlineAttrubute() {
      const offset = (this.trianglesLen - 1) * -10 + 122;
      return {
        y: offset,
        x: offset,
      };
    },
    isColorRemovable() {
      return this.trianglesLen > 1;
    },
    triangleAttributes() {
      const baseOffset = this.trianglesLen - 1 * 10;
      return this.triangleColors.map((color, i) => {
        const offset = (this.trianglesLen - i - 1) * 10;
        return {
          points: `${130 - offset} ${30 - offset}, ${30 - offset} ${130 - offset}, ${130 - offset} ${130 - offset}`,
          fill: `#${color}`,
          stroke: `#ffffff`,
          'stroke-width': `4px`,
        };
      }).reverse();
    },
  },
  mounted() {
    this.initializeParams();
  },
  methods: {
    removeColorHandler(index) {
      this.triangleColors = this.triangleColors.filter((_, i) => i !== index);
    },
    addColorHandler() {
      this.triangleColors.push(this.getRandomColor());
    },
    initializeParams() {
      this.triangleColors = [];
      this.mainline = this.getRandomNum();
      this.subline = this.getRandomName();
      for (var i = 0; i < 2; i++) {
        this.addColorHandler();
      }
    },
    getRandomColor() {
      return this.randomText('3456789abcde', 6);
    },
    getRandomNum() {
      return this.randomText('123456789', 2);
    },
    getRandomName() {
      const s = this.randomText('あいえおかきくけこさしそたちつてとなねのらるぱぴぷぽ', 3);
      return `${s}坂`;
    },
    randomText(chars, length) {
      return Array.from({length}).map(() =>
        chars.split('').sort(_ => Math.random() - 0.5)[0]
      ).join('');
    },
  },
}
</script>

<style>
body {
  padding: 20px;
  color: #2c3e50;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

h1,
p {
  margin: 0;
  padding: 0;
  font-size: 1em;
}

#app {
  max-width: 600px;
  margin: 0 auto;
}

.title {
  text-align: center;
  font-size: 22px;
}

.title::after {
  display: block;
  width: 40px;
  height: 3px;
  margin: 15px auto 0;
  content: '';
  background: #000000;
}

.logo {
  padding: 30px;
  margin: 0 auto;
  max-width: 250px;
  text-align: center;
}

.color-list {
  padding: 10px;
  background: #efefef;
}

.color {
  display: flex;
  align-items: center;
  margin-bottom: 5px;
  padding: 0 7px;
  background: #ffffff;
}

.color-chip {
  align-self: stretch;
  width: 20px;
}

.color-chip i {
  display: block;
  margin-top: 18px;
  margin-left: 10px;
  transform: rotate(45deg);
  border: 10px solid transparent;
}

.color-rm {
  padding: 5px 10px;
  color: #ffffff;
  border-radius: 3px;
  background: #aaaaaa;
  font-size: 10px;
  font-weight: bold;
  cursor: pointer;
}

.color-input {
  flex: 1;
}

.color-input input {
  box-sizing: border-box;
  width: 100%;
  padding: 10px;
  border: 0;
  outline: 0;
  font: inherit;
}

.add-color {
  margin: 20px 0 10px;
  text-align: center;
  font-size: 12px;
  cursor: pointer;
  text-decoration: underline;
}

.text-field {
  display: flex;
  align-items: center;
  border-bottom: 1px solid #d9d9d9;
  padding: 15px 10px;
  background: #efefef;
}

.text-field p {
  width: 70px;
  font-size: 12px;
  font-weight: bold;
}

.text-field input {
  flex: 1;
  padding: 10px;
  border: 0;
  outline: 0;
}

.random-shuffle {
  margin-bottom: 10px;
  text-align: center;
  text-decoration: underline;
  font-size: 13px;
  font-weight: bold;
  cursor: pointer;
}
</style>
