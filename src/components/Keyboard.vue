<template>
  <div class="keyboard" v-show="!showWord">
    <h1>EYES ON THE SCREEN</h1>
    <div class="row">
      <button class="key" ref="`" value="`" @click="keyclick">`</button>
      <button class="key" ref="1" value="1" @click="keyclick">1</button>
      <button class="key" ref="2" value="2" @click="keyclick">2</button>
      <button class="key" ref="3" value="3" @click="keyclick">3</button>
      <button class="key" ref="4" value="4" @click="keyclick">4</button>
      <button class="key" ref="5" value="5" @click="keyclick">5</button>
      <button class="key" ref="6" value="6" @click="keyclick">6</button>
      <button class="key" ref="7" value="7" @click="keyclick">7</button>
      <button class="key" ref="8" value="8" @click="keyclick">8</button>
      <button class="key" ref="9" value="9" @click="keyclick">9</button>
      <button class="key" ref="0" value="0" @click="keyclick">0</button>
      <button class="key" ref="-" value="-" @click="keyclick">-</button>
      <button class="key" :class="{}" ref="=" value="=" @click="keyclick">
        =
      </button>
      <button class="key" ref="BACKSPACE" value="BACKSPACE" @click="keyclick">
        DEL
      </button>
    </div>
    <div class="row">
      <button class="key utility" ref="TAB" value="TAB" @click="keyclick">
        Tab
      </button>
      <button class="key" ref="Q" value="Q" @click="keyclick">Q</button>
      <button class="key" ref="W" value="W" @click="keyclick">W</button>
      <button class="key" ref="E" value="E" @click="keyclick">E</button>
      <button class="key" ref="R" value="R" @click="keyclick">R</button>
      <button class="key" ref="T" value="T" @click="keyclick">T</button>
      <button class="key" ref="Y" value="Y" @click="keyclick">Y</button>
      <button class="key" ref="U" value="U" @click="keyclick">U</button>
      <button class="key" ref="I" value="I" @click="keyclick">I</button>
      <button class="key" ref="O" value="O" @click="keyclick">O</button>
      <button class="key" ref="P" value="P" @click="keyclick">P</button>
      <button class="key" ref="[" value="[" @click="keyclick">[</button>
      <button class="key" ref="]" value="]" @click="keyclick">]</button>
      <button class="key" ref="\" value="\" @click="keyclick">\</button>
    </div>
    <div class="row">
      <button
        class="key utility"
        ref="CAPSLOCK"
        value="CAPSLOCK"
        @click="keyclick"
      >
        CAPS
      </button>
      <button class="key" ref="A" value="A" @click="keyclick">A</button>
      <button class="key" ref="S" value="S" @click="keyclick">S</button>
      <button class="key" ref="D" value="D" @click="keyclick">D</button>
      <button class="key" ref="F" value="F" @click="keyclick">F</button>
      <button class="key" ref="G" value="G" @click="keyclick">G</button>
      <button class="key" ref="H" value="H" @click="keyclick">H</button>
      <button class="key" ref="J" value="J" @click="keyclick">J</button>
      <button class="key" ref="K" value="K" @click="keyclick">K</button>
      <button class="key" ref="L" value="L" @click="keyclick">L</button>
      <button class="key" ref=";" value=";" @click="keyclick">;</button>
      <button class="key" ref="'" value="'" @click="keyclick">'</button>
      <button class="key utility" ref="ENTER" value="ENTER" @click="keyclick">
        ENTER
      </button>
    </div>
    <div class="row">
      <button class="key utility" ref="SHIFT" value="SHIFT" @click="keyclick">
        SHIFT
      </button>
      <button class="key" ref="Z" value="Z" @click="keyclick">Z</button>
      <button class="key" ref="X" value="X" @click="keyclick">X</button>
      <button class="key" ref="C" value="C" @click="keyclick">C</button>
      <button class="key" ref="V" value="V" @click="keyclick">V</button>
      <button class="key" ref="B" value="B" @click="keyclick">B</button>
      <button class="key" ref="N" value="N" @click="keyclick">N</button>
      <button class="key" ref="M" value="M" @click="keyclick">M</button>
      <button class="key" ref="," value="," @click="keyclick">,</button>
      <button class="key" ref="." value="." @click="keyclick">.</button>
      <button class="key" ref="/" value="/" @click="keyclick">/</button>
      <button class="key utility" ref="SHIFT" value="SHIFT" @click="keyclick">
        SHIFT
      </button>
    </div>
  </div>
  <div v-show="showWord" class="phase-wrap">
    <div class="phrase">
      <h2>
        {{ phrase }}
      </h2>
      <button
        v-show="!finishGame"
        class="key"
        id="phrase-key"
        @click="showPhrase()"
      >
        Try again
      </button>
      <button v-show="finishGame" class="key" id="phrase-key" @click="reload()">
        Restart the game
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "KeyboardPage",
  data() {
    return {
      phrases: [
        "CHRISTMAS IS IN THE AIR",
        "LET IT SNOW",
        "MERRY AND BRIGHT",
        "PEACE, JOY, AND LOVE",
        "UNDER THE MISTLETOE",
        "JINGLE ALL THE WAY",
        "DREAMING OF A WHITE CHRISTMAS",
        "CHRISTMAS IS COMING",
        "BE MERRY AND SHINE BRIGHT",
        "MERRY CHRISTMAS",
      ],
      usedPhrases: [] as Array<string>,
      phrase: "",
      oneWord: "",
      letter: "",
      jiggleWord: "",
      showWord: false,
      finishGame: false,
      time: 0,
    };
  },
  mounted() {
    this.showPhrase();

    window.addEventListener("keypress", (element) => {
      this.keypress(element);
    });
  },
  computed: {},
  methods: {
    keyclick(element: any) {
      if (element?.target?.value) {
        if (this.letter !== element.target.value) {
          const addLetterRef: any = this.$refs[element.target.value];
          addLetterRef.classList.add("wrongJiggle");
          setTimeout(() => {
            addLetterRef.classList.remove("wrongJiggle");
          }, 2000);
        }

        if (this.letter === element.target.value) {
          const clickedButton: any = element.target.value;
          this.phrase = this.phrase.concat(clickedButton);

          const removeLetterRef: any = this.$refs[clickedButton];
          removeLetterRef.classList.remove("jiggle");

          this.jiggleOrder();
        }
      }

      if (this.letter === " ") {
        this.phrase = this.phrase.concat(this.letter);
        this.jiggleOrder();
      }
    },
    keypress(element: any) {
      const keyPress = String.fromCharCode(element.keyCode).toUpperCase();
      if (this.letter !== keyPress) {
        const addLetterRef: any = this.$refs[keyPress];
        if (addLetterRef) {
          addLetterRef.classList.add("wrongJiggle");
          setTimeout(() => {
            addLetterRef.classList.remove("wrongJiggle");
          }, 2000);
        }
      }

      if (this.letter === keyPress) {
        const clickedButton: any = keyPress;
        this.phrase = this.phrase.concat(clickedButton);

        const removeLetterRef: any = this.$refs[clickedButton];
        removeLetterRef.classList.remove("jiggle");

        this.jiggleOrder();
      }
    },
    showPhrase() {
      this.showWord = false;
      this.phrase = "";

      this.jiggleWord =
        this.phrases[Math.floor(Math.random() * this.phrases.length)];
      this.oneWord = this.jiggleWord;

      this.phrases.splice(this.phrases.indexOf(this.jiggleWord), 1);
      this.usedPhrases.push(this.jiggleWord);

      this.jiggleOrder();
    },
    jiggleOrder() {
      if (this.oneWord.length > 0) {
        this.letter = Array.from(this.oneWord)[0];
        this.oneWord = this.oneWord.substring(1);
        if (this.letter === " ") {
          this.keyclick(" ");
        }
        if (this.letter !== " ") {
          const addLetterRef: any = this.$refs[this.letter];
          addLetterRef.classList.add("jiggle");
        }
      }

      if (this.usedPhrases.includes(this.phrase)) {
        this.showWord = true;
      }

      if (this.phrases.length === 0) {
        this.finishGame = true;
      }
    },
    reload() {
      window.location.reload();
    },
  },
});
</script>

<style></style>
