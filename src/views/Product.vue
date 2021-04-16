<template>
  <div class="product">
      <HeaderKesra />

        <!-- Breadcrumb Section Begin -->
        <div class="breacrumb-section">
            <div class="container">
                <div class="row">
                    <div class="col-lg-12">
                        <div class="breadcrumb-text product-more text-left">
                            <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                            <span>Detail</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- Breadcrumb Section Begin -->

        <!-- Product Shop Section Begin -->
        <section class="product-shop spad page-details">
            <div class="container">
                <div class="row">
                    <div class="col-lg-12">
                        <div class="row">
                            <div class="col-lg-6">
                                <div class="product-pic-zoom">
                                    <img class="product-big-img" :src="foto_default" alt="" />
                                </div>
                                <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                                    <carousel class="product-thumbs-track ps-slider" :dots="false" :margin="10" :stagePadding="13" :autoplay="true" :nav="false">
                                        
                                        <div 
                                            v-for="thumb in productDetails.galleries" :key="thumb.id"
                                            class="pt" @click="changeFoto(thumb.photo)" 
                                            :class="thumb.photo==foto_default ? 'active' : '' ">

                                            <img :src="thumb.photo" alt="" />
                                        </div>

                                    </carousel>
                                </div>
                            </div>
                            <div class="col-lg-6">
                                <div class="product-details">
                                    <div class="pd-title text-left">
                                        <span>{{productDetails.type}}</span>
                                        <h3>{{productDetails.name}}</h3>
                                    </div>
                                    <div class="pd-desc text-left">
                                        <p>{{productDetails.description}}</p>
                                        <h4>{{productDetails.price | currency}},00</h4>
                                    </div>
                                    <div class="quantity">
                                        <router-link to="/checkout">
                                            <!-- set data ke dalam parameter fungsi -->
                                            <a @click="saveCart(
                                                productDetails.id, 
                                                productDetails.name, 
                                                productDetails.price, 
                                                productDetails.galleries[0].photo)" 
                                            href="#" class="primary-btn pd-cart">Masukkan Keranjang Belanja</a>
                                        </router-link>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
        <!-- Product Shop Section End -->

        <!-- separator start -->
        <div class="container">
          <div class="row">
            <div class="col mt-3 mb-1">
              <h2 class="text-divider"><span>Produk Terkait</span></h2>
            </div>
          </div>
        </div>
        <!-- separator end -->

        <RelatedKesra />

     <FooterKesra />
  </div>
</template>

<script>

// @ is an alias to /src
import carousel from 'vue-owl-carousel'
import axios from 'axios'

import HeaderKesra from '@/components/HeaderKesra.vue'
import FooterKesra from '@/components/FooterKesra.vue'
import RelatedKesra from '@/components/RelatedKesra.vue'

export default {
  name: 'Product',
  components: {
    carousel,
    HeaderKesra,
    FooterKesra,
    RelatedKesra
  },
  data(){
      return {
        foto_default: "",
        productDetails : [],
        userCart : []  
      }
  },

  methods : {
      changeFoto(url_image){
          this.foto_default = url_image;
      },
      setDataAndPicture(data){
          //replace object productDetails with data from API
          this.productDetails = data

        //   cekDefault = data.galleries[0].is_default

          //replace object foto_default with data from API (galleried)
          this.foto_default = data.galleries[0].photo
      },
      //fungsi mengubah objek ke data JSON dan menyimpannya ke data userCart
      saveCart(idProduct, nameProduct, priceProduct, photoProduct){

          //menyimpan parameter ke dalam variabel yang berisi objek
          var productStored = {
              "id" : idProduct,
              "name" : nameProduct,
              "price" : priceProduct,
              "photo" : photoProduct
          }
          
          //menyimpan variabel productStored ke dalam array userCart dalam bentuk JSON
          this.userCart.push(productStored)
          const parsed = JSON.stringify(this.userCart)

          //set userCart dengan data sebagai local storage
          localStorage.setItem('userCart',parsed)

      }
  },

  mounted(){

    //get local storage setelah fungsi di eksekusi dan data ada
    if (localStorage.getItem('userCart')) {
      try {
        this.userCart = JSON.parse(localStorage.getItem('userCart'));
      } catch(e) {
        localStorage.removeItem('userCart');
      }
    }

    axios
        //menentukan link api dengan get id tertentu 
        .get("https://kreska-admin.ceketer.com/api/products", {
            params : {
                id : this.$route.params.id
            }
        })
        .then(res => (this.setDataAndPicture(res.data.data)))
        .catch(err => console.log(err));
  },
  
}
</script>
