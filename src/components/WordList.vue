<template>
    <div class="wordlist-container">
        <div class="wordlist-columns">
            <h2>My Stored Words:</h2>
            <div class="wordlist">
                <ul>
                    <li v-for="(word, index) in words" :key="index">
                        <router-link :to="{name: 'Word', params: {
                          id: word.headword,
                          word: word.headword,
                          data: word
                        }}">{{ word }}</router-link>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'WordList',
  data() {
    return {
      words: this.$store.state.words.slice().reverse()
    }
  }
}
</script>

<style lang="scss" scoped>
.wordlist-container {
  margin-top: 60px;
  .wordlist-columns {
    width: 100%;
    display: flex;
    margin: 0 auto;
    text-align: left;
    align-items: center;

    h2 {
      flex: 0 0 15%;
      margin-top: 10px;
      text-align: center;
    }

    .wordlist {
      overflow: auto;
      flex: 0 0 85%;
      ul {
        list-style: none;
        margin: 0;
        padding: 0;
        height: calc(100vh-84px);
        display: flex;
        flex-wrap: wrap;
        margin-left: -10px;
        margin-top: -10px;
        li {
          text-align: center;
          background: cadetblue;
          padding: 10px;
          margin: 10px 5px 5px 10px;
          flex: 1 0 auto;
          box-sizing: border-box;
          max-width: 250px;
          a {
            color: white;
            text-decoration: none;
          }
        }
      }
    }
  }
}
</style>
