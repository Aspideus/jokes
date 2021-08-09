<template>
  <div class="main">
    <header class="header_logo">
      <div class="header_body">
        <h1>{{ msg }}</h1>
      </div>
    </header>

    <div class="search_window">
      <input type="text" v-model="search" placeholder="Введите слово для поиска среди анекдотов">
    </div>
    <!-- jokesDataList.jokes -->
    <div v-for="(jokeData,index) in filteredCards" :key="jokeData">
      <div class="joke_card" :id="jokeData.id">
        <div class="joke_text">
          <div><span>{{jokeData.joke}}{{jokeData.setup}}</span></div>
          <div><span>{{jokeData.delivery}}</span></div>
        </div>
        <button class="joke_like" @click = "add_like(index, jokeData);"><img src="../assets/like_w.png" alt="Like"/></button>
      </div>
      <br/>
    </div>
  </div>
</template>

<script>

export default {
  name: 'JokeLoad',

  props: {
    msg: String
  },
  data() {
    return {
      jokesDataList: [],
      search: ''
    };
  },
  created() {
    //localStorage.clear();
      fetch("https://v2.jokeapi.dev/joke/Any?amount=10")
        .then(response => response.json())
        .then(data => (this.jokesDataList = data));

  },
  computed: {
    filteredCards: function() {
      let search_string = this.search;
      let joke_list = this.jokesDataList.jokes;
      
      //на первой загрузке страницы он возвращает undefined, так и пусть они просто грузятся все
      if (joke_list !== undefined) {
        return joke_list.filter(function (jokeData) {

        if (localStorage.getItem(`joke${jokeData.id}`)) {
          //document.getElementById(jokeData.id).classList.add("liked_card");
          //console.log(document.getElementById(jokeData.id));
          if (document.getElementById(jokeData.id) != null)
            document.getElementById(jokeData.id).classList.add("liked_card");
        }

          if (search_string == undefined)
            return jokeData;
          else {
            if (jokeData.joke != undefined)
              return jokeData.joke.toLowerCase().match(search_string.toLowerCase());
            else { //проверим есть ли совпадение в сетапе
              if (jokeData.setup.toLowerCase().match(search_string.toLowerCase()) != null)
                return jokeData.setup.toLowerCase().match(search_string.toLowerCase());
              else
                return jokeData.delivery.toLowerCase().match(search_string.toLowerCase());
            }
          }
        });
      }
      return joke_list;
    }
  },
  methods: {
     add_like: function(number, joke) {

      let joke_text;
      if (joke.joke != undefined) 
        joke_text = joke.joke;
      else {
        joke_text = joke.setup + ' ' + joke.delivery;
      }
      
      document.getElementById(joke.id).classList.toggle("liked_card");

        if (localStorage.getItem(`joke${joke.id}`)) {
          //при перезагрузке страницы чтобы правильно отображалось (допустим поменяли порядок)
          if(localStorage.getItem(`joke${joke.id}`) != joke_text)
            localStorage.setItem(`joke${joke.id}`, joke_text);
          else
            localStorage.removeItem(`joke${joke.id}`);
        }
        else {
          localStorage.setItem(`joke${joke.id}`, joke_text);
        }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: var(--color-liked);
}
.main {
  max-width: 1200px;
  margin: 0px auto;
}
.joke_card {
  display: flex;
  background-color: var(--color-for-cards);
  border-radius: 15px;
  padding: 20px 40px;
  max-width: 1000px;
  margin: 0px auto;
  flex-wrap: wrap;
  justify-content: space-between;
  border: 1px solid var(--color-font);
}
.joke_card div {
  flex: 0 0 calc(100% - 60px);
  margin: 10px 0px;
  text-align: start;
  word-break: break-word;
  margin-right: 10px;
}
.joke_like {
  flex: 0 0 50px;
  height: 50px;
  align-self: center;
  border: none;
  background-color: transparent;
  outline: none;
}
.joke_like:hover {
  cursor: pointer;
}
.joke_like img {
  width: 100%;
}
.header_logo {
  position: absolute;
  top: 0;
  left: 0;
  height: 75px;
  background-color: var(--color-primary);
  color: var(--color-bg);
  width: 100%;
}
.header_body {
  position: relative;
  text-align: start;
  max-width: 1000px;
  margin: 0px auto;
  height: 100%;
  display: flex;
  align-items: center;
}
.header_body h1 {
  margin: 0;
}
.search_window {
  display: flex;
  padding: 20px 50px;
  width: 100%;
  max-width: 1200px;
  margin: 0px auto;
  max-width: 1080px;
}
.search_window input {
  height: 70px;
  width: 100%;
  padding: 0px 40px;
  font-size: 18px;
  background-color: var(--color-for-cards);
  border: 1px solid var(--color-font);
  /*margin: 0px 60px;*/
}
.liked_card {
  background-color: var(--color-liked);
}
@media (max-width: 1200px) { 
  .search_window {
    padding: 20px 0px;
  }
}
@media (max-width: 1080px) { 
  .header_body {
    max-width: 1080px;
    padding: 0px 40px;
  }

}
</style>
