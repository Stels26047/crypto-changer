<script>
  import Input from './components/Input.vue'
  import Selector from './components/Selector.vue'
  import CryptoConvert from 'crypto-convert';
  import Favourite from './components/Favourite.vue'

  const convert = new CryptoConvert();

  export default{
    components:{Input,Selector,Favourite},
    data(){
      return{
        amount:0,
        cryptoFirst: '',
        cryptoSecond: '',
        error: '',
        result: 0,
        favs: []
      }
    },
    methods:{

      changeAmount(val){
        this.amount = val
      },

      setCryptoFirst(val){
        this.cryptoFirst = val
      },

      setCryptoSecond(val){
        this.cryptoSecond = val
      },

      favorite(){
        if( this.cryptoFirst != '' && this.cryptoSecond != ''){
          if(this.cryptoFirst != this.cryptoSecond){
            this.favs.push({
              from: this.cryptoFirst,
              to: this.cryptoSecond
            })
          }
        }
      },

      getFromFavs(index){
        this.cryptoFirst = this.favs[index].from,
        this.cryptoSecond = this.favs[index].to
      },

      async convert(){
        if(this.amount <= 0){
          this.error = 'Число не введено или меньше 0';
          this.result = '';
          return;
        }else if(this.cryptoFirst == this.cryptoSecond){
          this.error = 'Выберите другую валюту';
          this.result = '';
          return;
        }else if(this.cryptoFirst == ''|| this.cryptoSecond == ''){
          this.error = 'Выберите валюту';
          this.result = '';
          return;
        }

        this.error = '';

        await convert.ready();

        if(this.cryptoFirst == 'BTC' && this.cryptoSecond == 'ETH')
          this.result = convert.BTC.ETH(this.amount);
        else if(this.cryptoFirst == 'BTC' && this.cryptoSecond == 'USDT')
          this.result = convert.BTC.USDT(this.amount);
        else if(this.cryptoFirst == 'ETH' && this.cryptoSecond == 'BTC')
          this.result = convert.ETH.BTC(this.amount);
        else if(this.cryptoFirst == 'ETH' && this.cryptoSecond == 'USDT')
          this.result = convert.ETH.USDT(this.amount);
        else if(this.cryptoFirst == 'USDT' && this.cryptoSecond == 'BTC')
          this.result = convert.USDT.BTC(this.amount);
        else if(this.cryptoFirst == 'USDT' && this.cryptoSecond == 'ETH')
          this.result = convert.USDT.ETH(this.amount);
      }
    }
  }
</script>

<template>
  <h1>CRYPTO</h1>
  <Input :changeAmount="changeAmount" :convert="convert" :favorite="favorite" />
  <p v-show="error != ''">{{ error }}</p>
  <p v-if="result != 0" className="result-text">{{ result }}</p>
  <Favourite :favs="favs" v-if="favs.length > 0" :getFromFavs="getFromFavs" />
  <div className="selectors">
    <Selector :setCrypto="setCryptoFirst" :cryptoNow="cryptoFirst"/>
    <Selector :setCrypto="setCryptoSecond" :cryptoNow="cryptoSecond"/>
  </div>
</template>

<style scoped>
  .selectors{
    display: flex;
    justify-content: space-around;
    width: 700px;
    margin: 0 auto;
  }
</style>
