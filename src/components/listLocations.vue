<template>
  <ul class="list">
    <li class="item"
        v-for="location of locations"
        :key="location.id">
      <card-location :location_info="location"></card-location>
    </li>
  </ul>
</template>

<script>
import CardLocation from "@/components/cardLocation";
export default {
  name: "listLocation",
  props: {
    locations_data: {
      type: Object
    },
  },
  data(){
    return{
      locations: [],
    }
  },
  components: {CardLocation},
  methods: {
    showList() {
      this.locations = this.locations_data
    },
  },
  watch: {
      locations_data() {
      this.showList()
    },
  }
}
</script>

<style scoped>
.list{
  list-style: none;
}

.item{
  margin-bottom: 56px;
}

@media screen and (min-width: 768px) {
  .list {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 32px;
  }

  .item {
    margin-bottom: 0;
  }
}
</style>