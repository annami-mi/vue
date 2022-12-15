<template>
  <header>
    <img class="logo" src="../img/logo.png">
  </header>
  <nav class="nav">
    <router-link
        :to="{name: 'main'}"
        class="nav__link"
        active-class="nav__link--active"
    >
      Эпизоды
    </router-link>
    <router-link
        :to="{name: 'charactersPage'}"
        class="nav__link"
        active-class="nav__link--active"
    >
      Персонажи
    </router-link>
    <router-link
        :to="{name: 'locationsPage'}"
        class="nav__link"
        active-class="nav__link--active"
    >
      Локации
    </router-link>
  </nav>
</template>

<script>
export default {
  name: "mainHeader",
  data(){
    return{
      activePage: ''
    }
  },
}
</script>

<style scoped>
header{
  text-align: center;
  margin-bottom: 16px;
}

.nav{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-bottom: 32px;
}


.nav__link{
  background: transparent;
  border-radius: 26px;
  color: #fff;
  border: 0;
  outline: none;
  padding: 13px 20px;
  font-size: 16px;
  font-weight: 500;
  cursor: pointer;
  display: inline-block;
  transition: 200ms;

  margin: 0 16px;
  text-decoration: none;
}

.nav__link--active{
  background: #39D870;
  color: #fff;
  box-shadow: 0 10px 46px #77DEB9;
}

.nav__link:hover{
  color: #39D870;
}

.nav__link--active:hover{
  background: #39D870;
  color: #fff;
  box-shadow: 0 10px 46px #77DEB9;
}

.logo{
  width: 60%;
}

@media screen and (min-width: 768px){
  header{
    margin-bottom: 32px;
  }
  .logo{
    width: 500px;
  }
  .nav{
    flex-direction: row;
    padding: 32px 0;
    margin-bottom: 0;
  }
}




</style>