<template>

  <div class="deal-container">
      <div class="grid-container">
        <div v-for="deal in deals" :key="deal.dealID" class="grid-item">
          <img :src="deal.thumb" :alt="deal.title" class="deal-image">
          <h2>{{ deal.title }}</h2>
          <div id="details">DETALHES</div>
          <div id="price-container">
            <p id="normal-price">$ {{ formatPrice(deal.normalPrice) }}</p>
            <p id="sale-price">$ {{ formatPrice(deal.salePrice) }}</p>
          </div>
          <p id="discount"> {{calculateDiscount(deal.savings)}}</p>
        </div>
      </div>
  </div>

</template>

<script>
export default {
  name: 'CardsPage',
  data() {
    return {
      deals: []
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
  color: #FFFFFF;
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
}

.grid-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #0B1641;
  border-radius: 5px;
  box-shadow:  0px 4px 4px 0 #00000025;
}

.deal-image {
  width: 100%;
  height: 147px;
  object-fit: cover;
  border-radius: 5px;
}

@media screen and (max-width: 768px) {

  .grid-container {
    display: grid;
    grid-template-columns: 1fr;
    padding: 20px;
  }
}

@media screen and (min-width: 769px) {
    
 .deal-container {
    padding: 9px 0 9px 130px;
  }
}

</style>