<template>
  <div id="app">
    <table class="table">
      <tr>
        <th class="header">KW 8</th>
        <th class="header">Montag </th>
        <th class="header">Dienstag</th>
        <th class="header">Mittwoch</th>
        <th class="header">Donnerstag</th>
        <th class="header">Freitag</th>
      </tr>
    </table>
    <table v-for="(row, index) in Rows" :key="index" class="table">
      <tr>
        <td class="header">{{ row.Name }}</td>
        <td v-for="(day, index) in row.Days" :key="index" class="cell">
          <div class="name">{{ getProduktName(day.ProductIds[0].ProductId) }}</div>
          <div class="price" >{{ getProduktPrice(day.ProductIds[0].ProductId) }} €</div>
          <div class="allergy">{{ getProduktAllergens(day.ProductIds[0].ProductId).toString() }}</div>
        </td>
      </tr>
    </table>
  </div>
</template>


<script>



export default {
  name: 'App',
  components: {
    
  },
  methods: {
    getProduktName(id){
      return this.Produkts[id].Name
    },
    getProduktPrice(id){
      return this.Produkts[id].Price.Betrag
    },
    getProduktAllergens(id){
      let ProduktsAllergens = [];
      this.Produkts[id].AllergenIds.forEach(el => {
        ProduktsAllergens.push(this.Allergens[el].Label)
      });
      return ProduktsAllergens 
    }
  },
  data(){
    return{
      API: "https://my.qnips.io/dbapi/ha",
      Produkts: [],
      Allergens: [],
      Rows: [],
    }
  },
  mounted(){
      fetch(this.API)
        .then((response) => {
            return response.json();
        })
        .then((data) => {
            this.Produkts = data.Products;
            this.Allergens = data.Allergens;
            this.Rows = data.Rows;
            console.log(data);
        });
    },
  
}
</script>

<style>
  #app {
    padding: 0;
    margin: 0;
  }
  .table {
    border-collapse: collapse;
    width: 80%; 
    padding: 0;
    margin: 0
  }
  
  .header, .cell {
    border: 1px solid black;
    text-align: center;
    padding: 0;
    margin: 0;
    width: 10%;
  }

  .header{
    background-color: rgba(0, 0, 0, 0.3); 
    font-weight: bold;
    font-size: 20px;
  }

  .price{
    font-weight: bold;
  }
</style>
