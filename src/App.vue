<template>
  <div id="app">
    <div class="back-button" v-on:click="previousQuote">
      <IconButton name="back"/>
    </div>
    <div class="middle-section">
      <div>
        <div class="quote-area">
          <span class="quote" v-html="quotes[currentQuote].split('-')[0]" />
          <br />
          <span class="author" v-html="quotes[currentQuote].split('-')[1]" />
        </div>
        <div v-on:click="download" class="download-button">
          <IconButton name="download"/>
        </div>
      </div>
    </div>
    <div class="forward-button" v-on:click="nextQuote">
      <IconButton name="forward"/>
    </div>
  </div>
</template>

<script>
import IconButton from './components/IconButton';

export default {
  name: 'app',
  components: {
    IconButton
  },
  data: () => ({
    quotes: [
      "That's one small step for a man, one giant leap for mankind - Neil Armstrong",
      "The Universe is under no obligation to make sense to you - Neil deGrasse Tyson",
      "I would like to die on Mars. Just not on impact - Elon Musk",
      "If we can conquer space, we can conquer childhood hunger - Buzz Aldrin",
      "The eternal silence of these infinite spaces frightens me - Blaise Pascal"
    ],
    currentQuote: 0,
  }),
  methods: {
    previousQuote() {
      if (this.currentQuote === 0) {
        this.currentQuote = this.quotes.length;
      } else {
        this.currentQuote--;
      }
    },
    nextQuote() {
      if (this.currentQuote === this.quotes.length - 1) {
        this.currentQuote = 0;
      } else {
        this.currentQuote++;
      }
    },
    fillWrappedText(context, text, x, y, maxWidth, lineHeight) {
      const words = text.split(' ');
      let line = '';
      const lines = [];

      for (let n = 0; n < words.length; n++) {
        const testLine = line + words[n] + ' ';
        const metrics = context.measureText(testLine);
        const testWidth = metrics.width;
        if (testWidth > maxWidth && n > 0) {
          lines.push(line);
          line = words[n] + ' ';
        }
        else {
          line = testLine;
        }
      }
      let offsettedY = y - (lines.length * lineHeight / 2);
      for (let i = 0; i < lines.length; i++) {
        context.fillText(lines[i], x, offsettedY);
        offsettedY += lineHeight;
      }
      context.fillText(line, x, offsettedY);
    },
    downloadCanvas(canvas, filename) {
      var lnk = document.createElement('a'), e;
      lnk.download = filename;
      lnk.href = canvas.toDataURL("image/png;base64");
      if (document.createEvent) {
        e = document.createEvent("MouseEvents");
        e.initMouseEvent("click", true, true, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
        lnk.dispatchEvent(e);
      } else if (lnk.fireEvent) {
        lnk.fireEvent("onclick");
      }
    },
    download() {
      const canvas = document.createElement('canvas');
      canvas.width = 500;
      canvas.height = 200;
      const ctx = canvas.getContext("2d");
      ctx.textAlign = "center";
      ctx.fillStyle = "red";
      ctx.font = "20px 'Avenir', Helvetica, Arial, sans-serif";
      this.fillWrappedText(ctx, this.quotes[this.currentQuote], 250, 100, 450, 25);
      this.downloadCanvas(canvas, "quote by - " + this.quotes[this.currentQuote].split('- ')[1]);
    },
  }
};
</script>

<style lang="scss">
@import '~vue-ionicons/ionicons.scss';

$break-small: 800px;

html, body {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  user-select: none;
  display: grid;
  height: 100%;
  grid-template-columns: 1fr 2fr 1fr;
  grid-template-rows: 1fr;
  @media screen and (max-width: $break-small) {
    grid-template-columns: 1fr;
    grid-template-rows: 25% 50% 25%;
  }

  #download-area {
    position: absolute;
    z-index: -1;
  }

  & > div {
    position: relative;
    height: 100%;
  }

  & > div > div {
    top: 50%;
    left: 50%;
    position: absolute;
    transform: translate(-50%, -50%);
  }

  .back-button {
    @media screen and (min-width: $break-small) {
      padding-left: 20px;
    }
  }

  .middle-section {

    & > div {
      width: 100%;
    }

    .quote-area {
      width: 90%;
      margin: auto;

      .quote {
        font-size: 35px;
        font-weight: bold
      }
    }

    .author {
      font-weight: bold;
      font-size: 25px;
      margin-top: 40px;
      display: inline-block;
      border-bottom: 3px dashed #444;
    }

    .download-button {
      @media screen and (min-width: $break-small) {
        position: absolute;
        margin: 0;
        top: 100%;
        margin-top: 50px;
        left: 50%;
        transform: translateX(-50%)
      }
    }
  }

  .forward-button {
    @media screen and (min-width: $break-small) {
      padding-right: 20px;
    }
  }
}
</style>
