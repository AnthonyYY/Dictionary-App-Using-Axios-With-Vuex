<template>
  <div class="home">
    <div class="input-container">
      <div class="input-group">
        <input type="text" @keydown.enter="getWord" v-model="word"/>
        <button @click="getWord" type="submit">Add Word</button>
      </div>
      <div class="word-meaning">
        {{ wordMeaning || error }}
        <div class="word-links" v-if="wordData">
          <router-link :to="{name: 'Word', params: {id: word, word: word, data: wordData}}">Read More</router-link>
          <router-link :to="{name: 'WordList'}">Word List</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import WordService from '@/services/WordService'
export default {
  name: 'Home',
  data() {
    return {
      word: '',
      wordData: '',
      error: ''
    }
  },
  computed: {
    wordMeaning() {
      if (this.wordData) {
        return this.wordData.senses[0].definition
      }
      return ''
    },
    words() {
      return this.$store.state.words
    }
  },
  methods: {
    async getWord(e) {
      if (this.word === '') {
        this.error = 'Please enter a word.'
        this.wordData = ''
        return false
      }
      const response = await WordService.getWord({ word: this.word })
      let responses = response.data.results
      if (responses.length === 0) {
        this.error = 'Your word could not be found and was not added.'
        this.wordData = ''
        return false
      }
      this.wordData = response.data.results[0]
      let words = this.words
      if (!words.filter(word => word.headword === this.word).length > 0) {
        this.$store.commit('addWord', this.word)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.home {
  display: flex;
  align-items: center;
  justify-content: center;
  height: calc(100vh);
  .input-container {
    max-width: 500px;
    width: 100%;
    .input-group {
      display: flex;
      justify-content: space-between;
      input {
        border-style: border-box;
        width: calc(100% - 150px);
        padding: 0.5em;
        font-size: 1em;
        border: 2px solid cornflowerblue;
        -webkit-appearance: none;
      }
      button {
        width: 150px;
        height: 50px;
        -webkit-appearance: none;
        background: cornflowerblue;
        color: white;
        text-transform: uppercase;
        border: none;
        font-weight: bold;
        font-size: 12pt;
      }
    }
  }
  .word-meaning {
    margin: 20px 0;
    font-size: 18px;
    font-weight: bold;
    .word-links {
      margin: 30px 0 20px 0;
      font-size: 14px;
      font-weight: regular;
      a {
        border: 1px solid royalblue;
        text-decoration: none;
        padding: 5px 12px 3px 12px;
        margin: 0 10px;
        text-transform: uppercase;
        &:focus {
          color: royalblue;
        }
      }
    }
  }
}
</style>
