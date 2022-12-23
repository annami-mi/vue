<template>
  <router-link
      class="card"
      :to="{name: 'locationItem', params: {id:location.id} }">

    <img class="card__img" src='../img/portal.png'>

    <div class="card__content">

      <p class="card__title">{{location.name}}</p>
      <p class="card__status">{{location.type}}</p>
      <p class="card__species">{{location.dimension}}</p>

    </div>

<!--    <div class="portal">-->
<!--      <div class="location">-->
<!--        <div class="location__name">{{location.name}}</div>-->
<!--        <div class="location__type">{{location.type}}</div>-->
<!--        <div class="location__type">{{location.dimension}}</div>-->
<!--      </div>-->
<!--    </div>-->
  </router-link>
</template>

<script>
export default {
  name: "cardLocation",
  props:{
    location_info:{
      type: Object
    }
  },
  data(){
    return{
      location: this.location_info
    }
  }
}
</script>

<style scoped>
.card{
  list-style: none;
  display: flex;
  cursor: pointer;
  transition: 200ms;
  /*overflow: hidden;*/
  border-radius: 16px;
  text-decoration: none;
  height: 100%;
  position: relative;
  top: 0;
  background-color: #424758;
}

.card:hover{
  top: -10px;
}

.card__img{
  width: 100px;
  min-height: 100px;
  object-fit: contain;
  display: block;
  margin-right: 24px;
  position: relative;
  left: 16px;
  top: -16px;
}

.card__content{
  background-color: #424758;
  padding: 16px;
  color: #E3E3E3;
  width: 100%;
  border-radius: 0 16px 16px 0;
}

.card__title{
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 16px;
}

.card__status{
  position: relative;
  margin-bottom: 16px;
  font-size: 14px;
}

.card__species{
  font-size: 14px;
}
</style>