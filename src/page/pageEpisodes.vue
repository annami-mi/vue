<template>

  <div class="headline">
    <groups-season @changeSeason="getCurrentSeason"></groups-season>

    <div class="search">
      <input-search @filteringItems="searchText"></input-search>
    </div>
  </div>

  <list-episodes :episodes_data="this.episodes"></list-episodes>
  <div v-if="notFound">not found</div>
  <buttons-pagination @click="showMoreItems" v-if="!hideButtonMore"></buttons-pagination>
</template>

<script>
import ListEpisodes from "@/components/listEpisodes";
import ButtonsPagination from "@/components/buttonsPagination";
import GroupsSeason from "@/components/groupsSeason";
import InputSearch from "@/components/inputSearch";

export default {
  name: "pageEpisodes",
  components: {InputSearch, GroupsSeason, ButtonsPagination, ListEpisodes},
  data(){
    return{
      episodes:[],
      episodesNextPage: '',
      season: 0,
      stopScroll: false,
      notFound: false,
      hideButtonMore: false,
      searchName: '',
      linkApi: 'https://rickandmortyapi.com/api/episode/',
    }
  },
  methods:{

    // Сортировка по сезонам
    getCurrentSeason(season){
      this.season = Number(season)
    },

    async changeSeason(){
      this.stopScroll = true
      let link;

      try {

        if(!this.season == 0){
          (this.searchName === '')
              ? link = (id = this.season) => `${this.linkApi}?episode=S0${id}`
              : link = (id = this.season, name = this.searchName) => `${this.linkApi}?episode=S0${id}&name=${name}`
        } else{
          (this.searchName === '')
              ? link = () => this.linkApi
              : link = (name = this.searchName) => `${this.linkApi}?name=${name}`
          console.log(link())
        }

        const response = await fetch(link());
        const episodes = await response.json();
        this.episodes = episodes.results;
        this.notFound = false;
        // показывать кнопку more если есть следующая страница в ответе от сервера
        (episodes.info.next) ? this.hideButtonMore = false : this.hideButtonMore = true
      } catch (e){
        this.notFound = true
      }
    },
    // Сортировка по сезонам


    // Бесконечная прокрутка START
    async showMoreItems(){
      try {
        const nextPage = this.episodesNextPage;
        const list = this.episodes;
        const response = await fetch(nextPage);
        const episodes = await response.json();
        this.episodesNextPage = episodes.info.next

        const all = list.concat(episodes.results);
        this.episodes = all
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
    async filterEpisodes(){
      this.stopScroll = true

      const search = this.searchName;
      const season = this.season;
      let link;

      if(!search == ''){
        (!season == 0)
            ? link = (name = search, id = season) => `${this.linkApi}?episode=S0${id}&name=${name}`
            : link = (name = search) => `${this.linkApi}?name=${name}`

        console.log(link())
      } else {
        (!season == 0)
            ? link = (id = season) => `${this.linkApi}?episode=S0${id}`
            : link = () => this.linkApi
      }

      console.log(!season == 0)
      console.log(link())

      let response;
      let episodes;

      try{
        response = await fetch(link());
        episodes = await response.json();
        this.episodes = episodes.results;
        this.episodesNextPage = episodes.info.next;
        this.notFound = false;
        (this.episodesNextPage) ? this.hideButtonMore = false : this.hideButtonMore = true
      } catch (e){
        this.notFound = true
      }
    },
  // Поиск по названию

  },

  async mounted() {
    const link = this.linkApi
    const response = await fetch(link);
    const episodes = await response.json();
    this.episodes = episodes.results;
    // при отрисовке запись следующей страницы без query параметров
    this.episodesNextPage = episodes.info.next;
  },
  watch:{
    season(){
      this.changeSeason();
    },
    searchName(){
      this.filterEpisodes();
    }
  }
}
</script>

<style scoped>

</style>