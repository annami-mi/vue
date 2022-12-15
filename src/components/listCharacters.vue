<template>
  <ul class="list">
    <li v-for="character of characters"
        :key="character.id"
        class="item"
    >
      <card-character :character_data="character"></card-character>
    </li>
  </ul>
</template>

<script>
import CardCharacter from "@/components/cardCharacter";
export default {
  name: "listCharacters",
  components: {CardCharacter},
  data(){
    return{
      characters: [],
    }
  },
  props:{
    characters_data:{
      type: Object
    }
  },
  methods:{
    showList(){
      this.characters = this.characters_data
    }
  },
  mounted() {
    this.characters = this.characters_data
  },
  watch:{
    characters_data(){
      this.showList()
    }
  },
}
</script>

<style scoped>
.list{
  list-style: none;
}
.item{
  margin-bottom: 24px;
}

@media screen and (min-width: 768px){
  .list{
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 24px;
  }
  .item{
    margin-bottom: 0;
  }
}
</style>