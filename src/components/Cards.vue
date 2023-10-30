<template>

  <div class="deal-container">
    <div class="search-container">
      <input v-model="searchQuery" type="text" placeholder="Procurar"/>
    </div>
    <div v-if="filteredDeals.length === 0" class="no-results">Nenhum resultado encontrado.</div>
      <div class="grid-container">
        <div v-for="deal in filteredDeals" :key="deal.dealID" class="grid-item">
          <img :src="deal.thumb" :alt="deal.title" class="deal-image">
          <div class="title-container">
            <h2>{{ deal.title }}</h2>
          </div>
          <div class="details-container">
            <div id="details-btn">DETALHES</div>
            <div id="price-container">
              <p id="normal-price">$ {{ formatPrice(deal.normalPrice) }}</p>
              <p id="sale-price">$ {{ formatPrice(deal.salePrice) }}</p>
            </div>
            <p id="discount"> {{calculateDiscount(deal.savings)}}</p>
          </div>
        </div>
      </div>
  </div>

</template>

<script>
export default {
  name: 'CardsPage',
  data() {
    return {
      deals: [],
      searchQuery: ''
    };
  },

  created() {
    fetch('https://www.cheapshark.com/api/1.0/deals?pageNumber=0&pageSize=12&storeID=1&onSale=1&AAA=1')
      .then(response => {
        if (!response.ok) {
          throw new Error('Erro ao receber a resposta da rede.');
        }
        return response.json();
      })
      .then(data => {
        console.log(data);
        this.deals = data;
      })
      .catch(error => {
        console.error('Houve um problema com a sua solicitação: ', error);
      });
  },

  computed: {
  filteredDeals() {
    if (this.searchQuery) {
      return this.deals.filter(deal =>
        deal.title.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    } else {
      return this.deals;
    }
  }
},

  methods: {
    formatPrice(price) {
      return price.toString().replace('.', ',')
    },

    calculateDiscount(savings) {
      const discount = Math.round(savings);
      if (discount === 100) {
        return "Grátis";
      }  
      return (`-${ discount }%`);
    },

  }

}

</script>

<style>

* {
  box-sizing: border-box;
  margin: 0;
  font-family: 'Roboto', sans-serif;
  color: #FFFFFF;
}

h2 {
  font-size: 24px;
  font-weight: 300;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.title-container {
  display: inline;
  width: 100%;
  padding: 9px 16px 0 16px;
}

.deal-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 1180px;
}

.grid-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;
  margin-bottom: 20px;
  padding-left: 130px;
}

.grid-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #0B1641;
  border-radius: 5px;
  box-shadow:  0px 4px 4px 0 #00000025;
  width: 380px;
  height: 251px;
}

.deal-image {
  width: 100%;
  height: 147px;
  object-fit: cover;
  border-radius: 5px;
}

.details-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  align-items: center;
  width: 100%;
  padding: 16px;
  
}

#details-btn {
  background-color: #C70160;
  padding: 10px;
  border-radius: 8px;
  font-size: 18px;
  font-weight: 700;
  text-align: center;
}

#normal-price {
  font-size: 12px;
  font-weight: 100;
  text-decoration: line-through;
  text-align: right;
  padding-right: 10px;
}

#sale-price {
  font-size: 18px;  padding: 0 20px;
  font-weight: 700;
  text-align: right;
  padding-right: 10px;
}

#discount {
  background-color: #16857B;
  padding: 10px;
  border-radius: 8px;
  font-size: 18px;
  font-weight: 700;
  text-align: center;
}

.search-container {
  display: flex;
  width: 100%;
  padding: 10px 0 30px 0;
}

input[type=text] {
  width: 380px;
  height: 50px;
  font-size: 18px;
  font-weight: 100;
  color: #FFFFFF;
  background-color: #0B1641;
  background-image: url('../assets/searchicon.png');
  background-position: 20px 15px;
  background-repeat: no-repeat;
  padding-left: 50px;
  border: none;
  border-radius: 8px;
  box-shadow:  0px 4px 4px 0 #00000025;
}

.no-results {
  text-align: center;
  margin-top: 20px;
  font-style: italic;
}

::placeholder {
  color: #ffffff;
}

@media screen and (max-width: 768px) {

  .deal-container {
    max-width: 100%;
}

  .grid-container {
    display: grid;
    grid-template-columns: 1fr;
    padding: 20px;
  }

  .grid-item {
    width: 340px;
    height: 251px;
  }

  .search-container {
    padding: 12px 0 10px 10px;
  }

  input[type=text] {
    width: 200px;
    height: 50px;
  }
}

@media screen and (min-width: 769px) {
    
 .deal-container {
    padding: 9px 0 9px 130px;
  }
}

</style>
