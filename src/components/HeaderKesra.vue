<template>

  <!-- Header Section Begin -->
    <header class="header-section">
      <div class="container">
        <div class="inner-header">
          <div class="row">
            <div class="col-lg-2 col-md-2 align-self-center">
              <div class="logo">
                <a href="./index.html">
                  <img src="img/logo_website_shayna.png" alt="" />
                </a>
              </div>
            </div>
            <div class="col-lg-7 col-md-7 align-self-center"></div>
            <div class="col-lg-3 text-right col-md-3 align-self-center">
              <ul class="nav-right">
                <li class="cart-icon">
                  Keranjang Belanja &nbsp;
                  <a href="#">
                    <i class="icon_bag_alt"></i>
                    <span>{{userCart.length}}</span>
                  </a>

                  <div class="cart-hover">
                    
                    <div v-if="userCart.length>0" class="select-items">
                      <table>
                        <!-- mengeluarkan data dari userCart yang telah terdeklarasi sebagai localstorage -->
                        <tbody>
                          <tr v-for="item in userCart" :key="item.id">
                            <td class="si-pic">
                              <img :src="item.photo" alt="" class="img-cart" />
                            </td>
                            <td class="si-text">
                              <div class="product-selected">
                                <p>{{item.price| currency}},00</p>
                                <h6>{{item.name}}</h6>
                              </div>
                            </td>
                            <td @click="removeItem(item.id)" class="si-close">
                              <i class="ti-close"></i>
                            </td>
                          </tr>
                        </tbody>
                      </table>

                      <div class="select-total">
                         <span><b>total:</b></span>
                         <h5><b>{{priceTotal | currency}},00</b></h5>
                       </div>

                      <div class="select-button">
                         <!-- <a href="#" class="primary-btn view-card">VIEW CARD</a> -->
                         <router-link to="/checkout">
                          <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>
                         </router-link>
                      </div>
                    </div>
                  
                    <div v-else class="select-items">
                      <tbody>
                        <tr>
                          <td>Keranjang Kosong, Yuk Belanja</td>
                        </tr>
                      </tbody>
                    </div>

                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </header>
 <!-- Header End -->

</template>

<script>
export default {
    
    name: 'HeaderKesra',
    
    //mengambil data dari komponen produk
    data(){
      return {
        userCart : []  
      }
    },
    methods: {
      removeItem(idx){
      // cari tau id dari item yang akan dihapus
        let userCartStore = JSON.parse(localStorage.getItem('userCart'))
        let itemUserCart  = userCartStore.map(itemUserCart => itemUserCart.id)

      // cocokan idx item dengan id yang ada di cart
        let index = itemUserCart.findIndex(id => id == idx)
        this.userCart.splice(index, 1)

      // kirim kembali datanya
        const parsed = JSON.stringify(this.userCart)
        localStorage.setItem('userCart',parsed)
        window.location.reload()
      
        
        // this.userCart.splice(index, 1)
        // const parsed = JSON.stringify(this.userCart)
        // localStorage.setItem('userCart',parsed)
        // window.location.reload()
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
      }
    }
}
</script>
<style scoped>
.checkout-btn{
  width: 290px;
}
.img-cart{
  width: 100px;
  height: 80px;
}
</style>