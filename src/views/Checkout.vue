<template>
 <div class="checkout">

  <HeaderKesra/>

  <!-- Breadcrumb Section Begin -->
     <div class="breacrumb-section">
         <div class="container">
             <div class="row">
                 <div class="col-lg-12">
                     <div class="breadcrumb-text product-more text-left">
                         <router-link to="/"><i class="fa fa-home"></i>Home</router-link>
                         <span>Checkout</span>
                     </div>
                 </div>
             </div>
         </div>
     </div>
  <!-- Breadcrumb Section Begin -->

  <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
        <div class="container">
            <div class="row">
                <div class="col-lg-8">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="cart-table">
                                <table>
                                    <thead>
                                        <tr>
                                            <th>Gambar</th>
                                            <th class="p-name text-center">Nama Produk</th>
                                            <th>Harga</th>
                                            <th>Aksi</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="item in userCart" :key="item.id">
                                            <td class="cart-pic first-row">
                                                <img :src="item.photo" />
                                            </td>
                                            <td class="cart-title first-row text-center">
                                                <h5>{{item.name}}</h5>
                                            </td>
                                            <td class="p-price first-row">{{item.price | currency}},00</td>
                                            <td class="delete-item">
                                                <a href="#">
                                                    <i @click="removeItem(item.index)" class="material-icons">close</i>
                                                </a>
                                            </td>
                                        </tr>
                                        
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        <div class="col-lg-8 text-left">
                            <h4 class="mb-4">
                                Informasi Pembeli:
                            </h4>
                            <div class="user-checkout">
                                <form>
                                    <div class="form-group">
                                        <label for="namaLengkap">Nama lengkap</label>
                                        <input type="text" class="form-control" id="namaLengkap"
                                            aria-describedby="namaHelp" placeholder="Masukan Nama"
                                            v-model="customerInfo.name" /> 
                                            <!-- v-model berfungsi untuk mengisikan inputan ke data customerInfo yang telah disiapkan -->
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">Alamat email</label>
                                        <input type="email" class="form-control" id="emailAddress"
                                            aria-describedby="emailHelp" placeholder="Masukan Email"
                                            v-model="customerInfo.email" />
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">No. HP</label>
                                        <input type="text" class="form-control" id="noHP" aria-describedby="noHPHelp"
                                            placeholder="Masukan No. HP"
                                            v-model="customerInfo.number" />
                                    </div>
                                    <div class="form-group">
                                        <label for="alamatLengkap">Alamat Lengkap</label>
                                        <textarea class="form-control" id="alamatLengkap" rows="3" v-model="customerInfo.address"></textarea>
                                    </div>

                                     <button @click="checkout()" class="primary-btn" type="button">Sudah Membayar</button>

                                </form>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="proceed-checkout text-left">
                                <div class="proceed-btn">Detail Transaksi</div>
                                <ul>
                                    <!-- <li class="subtotal">ID Transaksi <span>#SH12000</span></li> -->
                                    <li class="subtotal mt-3">Total <span>{{priceTotal | currency}},00</span></li>
                                    <li class="subtotal mt-3">Pajak <span>10%</span></li>
                                    <li class="subtotal mt-3">Total Biaya <span>{{priceFinal | currency}},00</span></li>
                                    <li class="subtotal mt-3">Bank Transfer <span>Mandiri</span></li>
                                    <li class="subtotal mt-3">No. Rekening <span>2208 1996 1403</span></li>
                                    <li class="subtotal mt-3">Nama Penerima <span>Alia</span></li>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Shopping Cart Section End -->
    
 </div>
</template>

<script>

import HeaderKesra from '@/components/HeaderKesra.vue'
import axios from 'axios'

export default {
 name: 'Checkout',
 components:{
     HeaderKesra,
 },
 data(){
    return {
        userCart : [],
        customerInfo : {
            name: '',
            email: '',
            number: '',
            address: '',
        }
      }
    },
    
    methods: {
      
      removeItem(index){
        this.userCart.splice(index, 1)
        const parsed = JSON.stringify(this.userCart)
        localStorage.setItem('userCart',parsed)

        window.location.reload()
      },
      
      checkout(){
        //mengambil data id barang belian
          let productIds = this.userCart.map(function(product){
              return product.id
          })

        //menampung data seluruhnya untuk oper ke API 
          let checkoutData = {
              'name' : this.customerInfo.name,
              'email' : this.customerInfo.email,
              'number' : this.customerInfo.number,
              'address': this.customerInfo.address,
              'transaction_total' : this.priceFinal,
              'transaction_status' : "PENDING",
              'transaction_details' : productIds
          }
        
          axios
            .post("https://kreska-admin.ceketer.com/api/checkout", checkoutData)
            .then(() => this.$router.push('sukses'))
            .catch(err => console.log(err))
      }

    },

    //mengambil data dari local storage
    mounted(){

      if (localStorage.getItem('userCart')) {
        try {
          this.userCart = JSON.parse(localStorage.getItem('userCart'));
        } catch(e) {
          localStorage.removeItem('userCart');
        }
      }
    },

    computed: {
      priceTotal(){
        return this.userCart.reduce(function(items, data){
            return items + data.price
        }, 0);
      },
      addTax(){
          return this.priceTotal * (10/100)
      },
      priceFinal(){
          return this.priceTotal + this.addTax
      }
    }

}
</script>

<style scoped>
.primary-btn{
    border: 0px;
}
</style>