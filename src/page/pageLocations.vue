<template>

  <div class="headline">
    <div class="search">
      <input-search @filteringItems="searchText"></input-search>
    </div>
  </div>

  <list-location :locations_data="this.locations"></list-location>
  <div v-if="notFound">not found</div>
  <buttons-pagination @click="showMoreItems" v-if="!hideButtonMore"></buttons-pagination>
</template>

<script>
import ListLocation from "@/components/listLocations";
import ButtonsPagination from "@/components/buttonsPagination";
import InputSearch from "@/components/inputSearch";
export default {
  name: "pageLocation",
  components: {
    InputSearch,
    ButtonsPagination,
    ListLocation
  },
  data(){
    return{
      locations:[],
      locationsNextPage: '',
      hideButtonMore: false,
      searchName: '',
      stopScroll: false,
      linkApi: 'https://rickandmortyapi.com/api/location/',
      notFound: false
    }
  },
  methods:{
    // Бесконечная прокрутка START
    async showMoreItems(){
      try {
        const nextPage = this.locationsNextPage;
        const list = this.locations;
        const response = await fetch(nextPage);
        const locations = await response.json();
        this.locationsNextPage = locations.info.next

        const all = list.concat(locations.results);
        this.locations = all
        this.hideButtonMore = true
        this.listenerScroll();

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

    async filterLocations(){
      const search = this.searchName;
      let link;

      (!search == '' )
          ? link = (name = search) => `${this.linkApi}?name=${name}`
          : link = () => this.linkApi

      let response;
      let locations;

      try{
        response = await fetch(link());
        locations = await response.json();
        this.locations = locations.results;
        this.locationsNextPage = locations.info.next;
        this.notFound = false;
        // показывать кнопку more если есть следующая страница в ответе от сервера
        (this.locationsNextPage) ? this.hideButtonMore = false : this.hideButtonMore = true
      } catch (e){
        this.notFound = true
      }
    },
    // Поиск по названию
  },
  async mounted() {
    const linkApiLoc = () => `https://rickandmortyapi.com/api/location`;
    const resLoc = await fetch(linkApiLoc());
    const locations = await resLoc.json();
    this.locations = locations.results;

    // при отрисовке запись следующей страницы без query параметров
    this.locationsNextPage = locations.info.next;
  },
  watch:{
    searchName(){
      this.filterLocations();
    }
  }
}
</script>

<style scoped>

</style>