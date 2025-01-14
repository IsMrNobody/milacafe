<template>
  <v-container>
    <!-- Search Component -->
    <!-- <search-products @search="handleSearch" /> -->

    <!-- Main Product Display -->
    <v-card class="mb-6 pa-4 transparent" elevation="2">
      <v-row>
        <v-col cols="12" md="6" class="d-flex justify-center align-center">
          <v-img
            :src="selectedProduct.image"
            max-width="400"
            contain
            class="rounded-lg"
          ></v-img>
        </v-col>

        <v-col cols="12" md="6">
          <search-products @search="handleSearch" />
          <div class="text-h4 font-weight-bold mb-2">{{ selectedProduct.name }}</div>
          <div class="text-subtitle-1 mb-4">{{ selectedProduct.description }}</div>
          <div class="text-h5 secondary--text mb-4">${{ selectedProduct.price }}</div>
          <v-btn color="white" outlined elevation="2" @click="addToCart()">
            Ordenar Ahora
          </v-btn>
        </v-col>
      </v-row>
      <v-row> 
        <v-card class="pa-4 transparent">
          <v-slide-group
            show-arrows
            class="pa-4"
          >
            <v-slide-item
              v-for="(product, index) in filteredProducts"
              :key="index"
              v-slot="{ active }"
            >
              <v-card
                class="ma-4"
                width="200"
                :class="{ 'on-hover': active }"
                @click="selectProduct(product)"
                >
                <v-card-title class="text-subtitle-1">
                  {{ product.name }}
                </v-card-title>
                <v-card-subtitle>
                  ${{ product.price }}
                </v-card-subtitle>
                <v-img
                  :src="product.image"
                  height="150"
                  cover
                ></v-img>
              </v-card>
            </v-slide-item>
          </v-slide-group>
        </v-card>
      </v-row>
    </v-card>
  </v-container>
</template>

<script>
import SearchProducts from '../components/SearchProducts.vue'

export default {
  name: 'CombosPage',
  components: {
    SearchProducts
  },
  layout: 'store',
  data() {
    return {
      selectedProduct: null,
      searchTerm: '',
      products: [
    {
      day: "Lunes",
      name: "Desayuno Americano",
      description: "Deliciosas panquecas acompañadas de syrop de maple huevo revuelto y tocineta crujiente con una bebida y de postre una galleta",
      price: 5.99,
      availability: "7:30am to 11:30am",
      image: 'https://cdn0.recetasgratis.net/es/posts/0/9/1/panquecas_34190_600_square.jpg'
    },
    {
      day: "Martes",
      name: "Sandwich clásico",
      description: "Con tres capas de pan, jamón y queso, lechuga y tomate más salsas básicas con una bebida y de postre una galleta",
      price: 4.50,
      availability: "7:30am to 11:30am",
      image: 'https://www.cocinadelirante.com/800x600/filters:format(webp):quality(75)/sites/default/files/images/2018/04/club-sandwich.jpg'
    },
    {
      day:"Miércoles",
      name:"Tortilla de huevo",
      description:"Con una corteza crujiente de queso mozzarella, rellena de aguacate y tomate coronada con cebollín acompañado de una infusión de jamaica",
      price:4,
      availability:"7:30am to 11:30am",
      image: 'https://res.cloudinary.com/dku13l2ep/image/upload/v1722271728/JARTATE/Ciudad/maracay/milacafe/logo/color-white_lbbdjw.png'
    },
    {
      day: "Jueves",
      name: "2 empanadas",
      description: "2 empanadas de tu preferencia más un papelon con limón",
      price: 3,
      availability:"7:30am to 11:30am",
      image: 'https://res.cloudinary.com/dku13l2ep/image/upload/v1722271728/JARTATE/Ciudad/maracay/milacafe/logo/color-white_lbbdjw.png'
    },
    {
      day: "Viernes",
      name: "Hamburguesa Cheeseburger",
      description:"Una hamburguesa cheeseburger con 100gr de papas fritas más un refresco",
      price:4.99,
      availability: "12:00pm to 6:00pm",
      image: 'https://res.cloudinary.com/dku13l2ep/image/upload/v1722271728/JARTATE/Ciudad/maracay/milacafe/logo/color-white_lbbdjw.png'
    },
    {
        day:"Sábado",
        name:"Combo Familiar",
        description:"1 cheeseburger, 1 bacon cheeseburger, 1 Oklahoma Burger, 1 americana Burger, 1 Mila Burger + 300gr de papas fritas + 1 Cocacola de 2lt",
        price:29.99,
        availability:"12:00pm to 6:00pm",
        image: 'https://res.cloudinary.com/dku13l2ep/image/upload/v1722271728/JARTATE/Ciudad/maracay/milacafe/logo/color-white_lbbdjw.png'
    },
      {
        day:"Sábado",
        name:"Combo Premium",
        description:"3 Bacon cheeseburger + refresco 2 lt",
        price:17.99,
        availability:"12:00pm to 6:00pm",
        image: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQV8QN69Uvuy76oXnHBHZkuvl2Gv_IfUTedAQ&sy'
    },
      {
       day:"Sábado",
        name:"Combo Parejas",
        description:"2 doble cheeseburger + Cocacola de 1,5lt + galleta",
        price:15.99,
        availability:"12:00pm to 6:00pm",
        image: 'https://res.cloudinary.com/dku13l2ep/image/upload/v1722271728/JARTATE/Ciudad/maracay/milacafe/logo/color-white_lbbdjw.png'
    }
  ]
    }
  },
  computed: {
    filteredProducts() {
      if (!this.searchTerm) return this.products
      
      return this.products.filter(product => {
        return product.name.toLowerCase().includes(this.searchTerm) ||
               product.description.toLowerCase().includes(this.searchTerm)
      })
    }
  },
  created() {
    // Set the first product as selected by default
    this.selectedProduct = this.products[0]
  },
  methods: {
    selectProduct(product) {
      this.selectedProduct = product
    },
    handleSearch(query) {
      this.searchTerm = query
      // If there are filtered results and current selection is not in filtered results,
      // update the selected product
      if (this.filteredProducts.length > 0 && 
          !this.filteredProducts.includes(this.selectedProduct)) {
        this.selectedProduct = this.filteredProducts[0]
      }
    },
    addToCart() {
      const data = {
          ...this.selectedProduct,
          totalProducto: this.selectedProduct.price,
          selectedPortion: {},
          cantidad: 1,
          img: {
            url: this.selectedProduct.image
          }
        }
        try  {
          this.$store.dispatch('carrito/addToCar', data)
        } catch (error) {
          console.log('aqui error', error)
        }
    }
  }
}
</script>

<style scoped>
.on-hover {
  opacity: 0.8;
}
</style>