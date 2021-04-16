<template>
     <!-- Women Banner Section Begin -->
    <section class="women-banner spad" id="#item">
        <div class="container-fluid">
            <div class="row">
                <!--start check data produk-->
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    
                    <carousel class="product-slider" :margin="25" :loop="false" :stagePadding="100" :autoplay="true" :nav="false" autoplayHoverPause="true">

                        <!-- start perulangan -->
                        <div class="product-item" v-for="itemProduct in products"  v-bind:key="itemProduct.id">
                            
                            <!-- start check jumlah produk > 0 -->
                            <div v-if="itemProduct.quantity>0">

                                <div class="pi-pic">
                                    <img v-bind:src="itemProduct.galleries[0].photo" alt="" />
                                    <ul>
                                        <li @click="saveCart(
                                                itemProduct.id, 
                                                itemProduct.name,
                                                itemProduct.price,
                                                itemProduct.galleries[0].photo)" class="w-icon active"> 
                                            <a href="#">+<i class="icon_bag_alt"></i></a>
                                        </li>
                                        <li class="quick-view"><router-link v-bind:to="'/product/'+itemProduct.id">Lihat Detail</router-link></li>
                                    </ul>
                                </div>

                                <div class="pi-text">
                                    <div class="catagory-name">{{itemProduct.type}}</div>
                                    <a href="#">
                                        <h5 class="mb-1 mt-3"><b>{{itemProduct.name}}</b></h5>
                                    </a>
                                    <div class="product-price">
                                        {{itemProduct.price | currency}},00
                                    </div>
                                </div>

                            </div>
                            <!-- end check jumlah produk > 0 -->

                            <!-- start else check jumlah produk > 0 -->
                            <div v-else>
                                <div class="pi-pic">
                                    <img style="opacity : 0.4;" v-bind:src="itemProduct.galleries[0].photo" alt="" />
                                    <ul>
                                        <li class="quick-view"><a>Produk Habis</a></li>
                                    </ul>
                                </div>
                               
                                <div class="pi-text">
                                    <div class="catagory-name">{{itemProduct.type}}</div>
                                    <a>
                                        <h5 class="mb-1 mt-3 text-muted"><b>{{itemProduct.name}}</b></h5>
                                    </a>
                                    <div class="product-price text-muted">
                                        {{itemProduct.price | currency}},00
                                    </div>
                                </div>
                            </div>
                            <!-- end else -->

                        </div>
                        <!-- end perulangan -->

                    </carousel>

                </div>
               <!--end check data produk-->

               <!-- else check data produk -->
                <div class="col-lg-12" v-else>
                   <p>Ada kesalahan dalam data produk</p>
                </div>

            </div>

        </div>
    </section>
    <!-- Women Banner Section End -->
</template>

<script>

import carousel from 'vue-owl-carousel'
import axios from 'axios'

export default {
    name: 'ItemKesra',
    components: {
     carousel
    },

    data(){
        return{
            products : [],
            userCart : []
        };
    },
    
    methods:{
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

          window.location.reload()
      }
    },

    mounted(){
    // nggak perlu isi mounted di bawah ini karena tidak ada data yang dimanfaatkan di komponen ini,
    // kalau pada view produk itu ada data yang dimanfaatkan yaitu untuk mengisi daftar tabel list belanjaan
    // kalau disini kan udah di backup langsung sama header

    // update april: ini digunkan untuk mengirim data setelah reload

        if (localStorage.getItem('userCart')) {
             try {
                 this.userCart = JSON.parse(localStorage.getItem('userCart'));
             } catch (e) {
                 localStorage.removeItem('userCart');
             }
         }
        
        axios
         .get("https://kreska-admin.ceketer.com/api/products")
         .then(res => (this.products = res.data.data.data))
         .catch(err => console.log(err));
    }
}
</script>

<style scoped>
.top-left {
  position: absolute;
  top: 8px;
  left: 16px;
}
</style>