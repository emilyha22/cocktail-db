<template>
  <div id="app">
  <router-view></router-view>
  <h1>{{ message }}</h1>
  <input type="text" v-model="drink" placeholder="Search for a drink..."/>
  <button v-on:click="searchDrink">Search</button>

  <ul class="cocktail">
  <li v-for="(cocktail,index) in drinkList" v-bind:key="cocktail">
    <img :src=cocktail.strDrinkThumb>
    <div>
      <h2>{{cocktail.strDrink}}</h2>
      <div><button v-on:click="displayInfo(cocktail,index)">See More</button></div>
      </div>
  </li>
  </ul>

  <div class="modal">
    <ul>
      <li v-for="item in currentDrinkDisplay" v-bind:key="item">
          <img :src=item.strDrinkThumb>
          <h1>{{ item.strDrink }}</h1>
          <h2>Ingredients:</h2>
          <h3>{{ item.strIngredient1 }}</h3>
          <h3>{{ item.strIngredient2 }}</h3>
          <h3>{{ item.strIngredient3 }}</h3>
      </li>
    </ul>
    <div><button v-on:click="modalOff()">Close</button></div>
  </div>




  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
    
  },

  data: function(){
    return{
      message: "Search for a Drink",
      drink:'',
      currentDrinkDisplay:[],
      drinkList:[],
      strDrink:'',
      strIngredient1:'',
      strIngredient2:'',
      strIngredient3:'',
    }
  },


  methods:{

    //search cocktail
    searchDrink(){ 
      fetch(`https://www.thecocktaildb.com/api/json/v1/1/search.php?s=${this.drink}`)
      .then(response => response.json())
      .then(data => {
 
        this.drinkList = data['drinks'];
      });
    },

    displayInfo(cocktail,index){
      this.strDrink = this.drinkList[index];
      //displays drink name
      console.log(this.strDrink['strDrink']);
      this.currentDrinkDisplay.push(this.drinkList[index]);
      console.log(this.currentDrinkDisplay);
      console.log(this.drinkList);
      document.querySelector('.modal').style.display = 'block';
    },

    modalOff(){
      document.querySelector('.modal').style.display = 'none';
      this.currentDrinkDisplay = [];
    }


  
}
}


</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

img{
  width: 200px;
}

.cocktail li{
  display: flex;
  align-items: center;
  margin-bottom: 30px;

}

.modal{
  width: 700px;
  position: fixed;
  top: 25%;
  left: 25%;
  background: whitesmoke;
  padding: 30px;
  border-radius:10px ;
  display: none;
}

li{
  list-style-type: none;
}

ul{
  padding: 0;
}
</style>
