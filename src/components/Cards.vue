<template>

  <div class="inputs-container">
    <div class="search-container">
      <input v-model="searchQuery" type="text" placeholder="Procurar"/>
    </div>
      <div class="order-container">
        <p id="order-by">Ordenar por:</p>
        <select v-model="sortBy">
          <option value="" disabled selected>Selecione</option>
          <option value="savings">% de Desconto</option>
          <option value="highestSalePrice">Maior Preço</option>
          <option value="lowestSalePrice">Menor Preço</option>
          <option value="title">Título</option>
        </select>
      </div>
  </div>

  <div class="deal-container">
    <div v-if="filteredDeals.length === 0" class="no-results">Nenhum resultado encontrado.</div>
      <div class="grid-container">
        <div v-for="deal in filteredDeals" :key="deal.dealID" class="grid-item">
          <img :src="deal.thumb" :alt="deal.title" class="deal-image">
          <div class="title-container">
            <h2>{{ deal.title }}</h2>
          </div>
          <div class="details-container">
            <div id="details-btn">DETALHES</div>
            <div id="prices-container">
              <p id="normal-price">$ {{ formatPrice(deal.normalPrice) }}</p>
              <p id="sale-price">$ {{ formatPrice(deal.salePrice) }}</p>
            </div>
            <p id="discount"> {{calculateDiscount(deal.savings)}}</p>
          </div>
        </div>
      </div>
    </div>
    <div class="container-btn">
      <button @click="loadMore" class="load-more-btn">
        Carregar mais
      </button>
    </div>

</template>

<script>
export default {
  name: 'CardsPage',
  
  data() {
    return {
      deals: [],
      searchQuery: '',
      sortBy: '',
      pageNumber: 0,
    };
  },

  created() {
    this.fetchDeals();
  },

  computed: {

  filteredDeals() {
    let filtered = this.deals;

      if (this.searchQuery) {
        filtered = filtered.filter(deal =>
          deal.title.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      } if (this.sortBy === 'savings') {
          filtered.sort((a, b) => b.savings - a.savings);
      } if (this.sortBy === 'highestSalePrice') {
          filtered.sort((a, b) => b.salePrice - a.salePrice);
      } if (this.sortBy === 'lowestSalePrice') {
          filtered.sort((a, b) => a.salePrice - b.salePrice);
      } if (this.sortBy === 'title') {
          filtered.sort((a, b) => a.title.localeCompare(b.title));
      }
        return filtered;
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

    fetchDeals() {
      const url = `https://www.cheapshark.com/api/1.0/deals?pageNumber=${this.pageNumber}&pageSize=12&storeID=1&onSale=1&AAA=1`;

      fetch(url)
        .then(response => {
          if (!response.ok) {
            throw new Error('Erro ao receber a resposta da rede.');
          }
          return response.json();
        })
        .then(data => {
          this.deals = [...this.deals, ...data];
          this.pageNumber += 1;
        })
        .catch(error => {
          console.error('Houve um problema com a sua solicitação: ', error);
        });
    },

    loadMore() {
      this.fetchDeals();
    },
  },
}

</script>

<style>

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
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
  font-size: 18px;  
  padding: 0 20px;
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

.inputs-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px 0 20px 130px;
  max-width: 1180px;
}

.search-container {
  display: flex;
  align-items: center;
}

input[type=text] {
  width: 380px;
  height: 50px;
  font-size: 18px;
  font-weight: 100;
  color: #FFFFFF;
  background-color: #0B1641;
  background-image: url('../assets/searchicon.png');
  background-position: 22px 16px;
  background-repeat: no-repeat;
  padding-left: 60px;
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

.order-container {
  display: flex;
  align-items: center;
}

select {
  width: 180px;
  height: 50px;
  font-size: 18px;
  font-weight: 100;
  background-color: #0B1641;
  background-image: url('../assets/expandmore.png');
  background-position: 150px 22px;
  background-repeat: no-repeat;
  padding: 0 20px;
  border: none;
  border-radius: 8px;
  box-shadow: 0px 4px 4px 0 #00000025;
  cursor: pointer;
  appearance: none;
}

select option {
  font-size: 18px;
}

.container-btn {
  display: flex;
  justify-content: center;
  margin: 10px 0 40px 130px;
  max-width: 1180px;
}

.load-more-btn {
  width: 380px;
  height: 50px;
  font-size: 18px;
  font-weight: 100;
  background-color: #0B1641;
  border-radius: 8px;
  box-shadow: 0px 4px 4px 0 #00000025;
  cursor: pointer;
  border: none;
}

@media screen and (max-width: 768px) {

  .container-btn  {
    display: flex;
    margin: 0 10px 40px 10px;
  }

  .inputs-container {
    display: flex;
    align-items: flex-end;
    margin: 0;
  }

  #order-by {
    font-size: 12px;
    padding: 5px;
  }

  .order-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-right: 10px ;
  }

  select {
    width: 140px;
    padding-left: 20px;
    background-position: 116px 22px;
  }

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
    height: 241px;
  }

  .search-container {
    padding: 12px 10px 0px 10px;
  }

  input[type=text] {
    width: 190px;
    height: 50px;
  }
}

@media screen and (min-width: 769px) {
    
 .deal-container {
    padding: 9px 0 9px 130px;
  }

  #order-by {
    font-size: 18px;
    font-weight: 700;
    padding-right: 10px;
  }
}

</style>
