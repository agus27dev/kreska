<template>
     
     <div class="d-flex success-checkout align-items-center justify-content-center">
         
        <div class="col col-lg-4 text-center">
            <img src="img/success.png" alt="" width="294">
            <h3 class="mt-4">
                Transaksi Tersimpan!
            </h3>
            <p class="mt-2">
                Silakan hubungi admin kami di WhatsApp <b>(089xxxxxxxxx)</b> untuk mengirim bukti pembayaran dan konfirmasi pesanan.
            </p>

            <p class="mt-0">
                Terimakasih telah mendukung kami untuk tetap berkreasi, SMK BISA!
            </p>

            <!-- <a href="index.html" class="primary-btn pd-cart mt-3">Belanja Lagi</a> -->
            <!-- <router-link to="/"> -->
                <a href="index.html" class="primary-btn">Belanja Lagi</a>
            <!-- </router-link> -->

        </div>
    </div>
</template>

<script>

export default {
    name: 'Sukses',

    data(){
        return {
        userCart : []  
      }
    },

    methods:{

        callFunction: function () {
           
            setTimeout(function () {
               this.localStorage.clear();
            }, 3000);
        }
        
    },

    mounted(){
      
      this.callFunction()

      if (localStorage.getItem('userCart')) {
        try {
          this.userCart = JSON.parse(localStorage.getItem('userCart'));
        } catch(e) {
          localStorage.removeItem('userCart');
        }
      }
        
    
    },
}
</script>

<style scoped>

</style>