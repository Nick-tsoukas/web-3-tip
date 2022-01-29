<template>
 <div>
  <HeaderNav/>
  <section class="w-full px-10 md:pl-36 md:pr-36 py-10">
    <h2 class="text-4xl pb-4">Hello ... </h2>
    <p class="text-lg">I'm working hard for you and would like a tip in some ether!!! {{name}}</p>
  </section>
  <section class="w-screen grid place-items-center">
    <div class="h-96 w-screen bg-green-400 md:w-96"></div>
  </section>
  <!-- tip form -->s
  <section v-if="wallet" class="bg-black text-white w-56 h-56 flex flex-col p-10 absolute right-20 bottom-10 justify-around">
    <p>If you like my code please send a tip</p>
    <input v-model="amount" class="border-2 border-black text-black" placeholder="00.1" type="number">
    <button class="text-lg text-black bg-green-400" @click="send" >Send Tip</button>
  </section>
  <section v-else class="bg-black text-white w-56 h-56 flex flex-col p-10 absolute right-20 bottom-10">
    <p>Would you like to install a digital wallet</p>
   <div class="flex">
      <a class="flex" href="https://metamask.io/"><img class="w-24" src="/mm-logo.svg" alt="">Install Metamask</a>
   </div>
  </section>
 </div>
</template>

<script>
export default {
  data(){
    return{
      name: 'Nick',
      wallet: false,
      amount: null
    }
  },
  mounted(){
    if(window.ethereum){
      this.wallet = true;
    }
  },
  methods: {
   async send(){
    window.web3 = new this.$Web3(window.ethereum);
    const wei = window.web3.utils.toWei(this.amount, 'ether')
      const accounts =  await window.ethereum.request({ method:'eth_requestAccounts'});
     if(accounts.length > 0) {
       window.ethereum.request({ 
         method: 'eth_sendTransaction',
         params: [{
           from: accounts[0],
           to: '0xCB9a884eF057f04C582bfDadaCB3B8e1A7d9E8D8',
           value: window.web3.utils.toHex(wei)
         }]
       })
     }

    }
  }
}
</script>
