<template>
  <div class="headline">
    <div class="search">
      <input-search @filteringItems="searchText"></input-search>
      <filter-checkbox @changeStatus="statusLive"></filter-checkbox>
    </div>
  </div>

  <list-characters :characters_data="this.characters"></list-characters>
  <div v-if="notFound">not found</div>
  <buttons-pagination @click="showMoreItems" v-if="!hideButtonMore"></buttons-pagination>

</template>

<script>
import ListCharacters from "@/components/listCharacters";
import ButtonsPagination from "@/components/buttonsPagination";
import InputSearch from "@/components/inputSearch";
import FilterCheckbox from "@/components/filterCheckbox";

export default {
  name: "pageCharacters",
  components: {FilterCheckbox, InputSearch, ButtonsPagination, ListCharacters},
  data(){
    return{
      characters:[],
      charactersNextPage: '',
      stopScroll: false,
      hideButtonMore: false,
      searchName: '',
      status:[],
      linkApi: 'https://rickandmortyapi.com/api/character/',
      notFound: false
    }
  },
  methods:{


    // Бесконечная прокрутка START
    async showMoreItems(){
      try {
        const nextPage = this.charactersNextPage;
        const list = this.characters;
        const response = await fetch(nextPage);
        const characters = await response.json();
        this.charactersNextPage = characters.info.next

        const all = list.concat(characters.results);
        this.characters = all
        this.hideButtonMore = true
        this.listenerScroll();
        this.stopScroll = false

      } catch (e){
        console.log('end')
      }
    },

    listenerScroll(){
      window.onscroll = () => {
        let bottomOfWindow = document.documentElement.scrollTop + window.innerHeight === document.documentElement.offsetHeight;
        if (bottomOfWindow && !this.stopScroll) {
          this.showMoreItems()
        }
      };
    },
    // Бесконечная прокрутка END

    // Поиск по названию
    searchText(text){
      this.searchName = text
    },

    async filterCharacters(){
      this.stopScroll = true

      const searchName = this.searchName;
      const status = this.status;
      let link;

      if(!searchName == '' ){
        (status == 'Alive' || status == 'Dead' || status == 'unknown' )
            ? link = (name = searchName, id = this.status) => `${this.linkApi}?status=${id}&name=${name}`
            : link = (name = searchName) => `${this.linkApi}?name=${name}`
      } else {
        (status == 'Alive' || status == 'Dead' || status == 'unknown')
            ? link = (id = this.status) => `${this.linkApi}?status=${id}`
            : link = () => this.linkApi
      }

      let response;
      let characters;

      try{
        response = await fetch(link());
        characters = await response.json();
        this.characters = characters.results;
        this.charactersNextPage = characters.info.next;
        this.notFound = false;
        // показывать кнопку more если есть следующая страница в ответе от сервера
        (this.charactersNextPage) ? this.hideButtonMore = false : this.hideButtonMore = true
      } catch (e){
        this.notFound = true
      }

    },
    // Поиск по названию

    // Фильтр по статусу
    statusLive(obj){
      this.status = obj
    },
    async filterOfStatus(){
      this.stopScroll = true

      // получение статуса из data
      const status = this.status;
      // хранинение новой ссылки API и полученого запроса json
      let link;
      let response;
      let characters;

      console.log(status)
        // проверка на наличие статуса и получение нужной ссылки
        if(status == 'Alive' || status == 'Dead' || status == 'unknown'){
          // проверка на наличие поискового запроса и формирование ссылки с нужными параметрами
          (this.searchName === '')
              ? link = (id = status) => `${this.linkApi}?status=${id}`
              : link = (id = status, name = this.searchName) => `${this.linkApi}?status=${id}&name=${name}`
        } else {
          (this.searchName === '')
              ? link = () => this.linkApi
              : link = (name = this.searchName) => `${this.linkApi}?name=${name}`
        }

      // перезаписываем данные по полученым массивам
      try {
        response = await fetch(link());
        characters = await response.json();
        this.characters = characters.results;
        this.charactersNextPage = characters.info.next;
        this.notFound = false;

        // показывать кнопку more если есть следующая страница в ответе от сервера
        (characters.info.next) ? this.hideButtonMore = false : this.hideButtonMore = true
      } catch (e) {
        this.notFound = true
      }

    },
    // Фильтр по статусу
  },

  async mounted() {
      const linkApi = this.linkApi;
      const response = await fetch(linkApi);
      const characters = await response.json();
      this.characters = characters.results;

      // при отрисовке запись следующей страницы без query параметров
      this.charactersNextPage = characters.info.next;
  },
  watch:{
    searchName(){
      this.filterCharacters();
    },
    status(){
      this.filterOfStatus();
    }
  }
}
</script>

<style scoped>

</style>