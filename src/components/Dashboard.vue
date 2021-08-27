<template>
    <div class="mx-auto max-w-7xl px-4 mt-10 sm:mt-14">
        <nav class="relative flex flex-wrap items-center justify-between px-2 py-3 bg-blue-500 mb-3">
            <div class="container px-4 mx-auto flex flex-wrap items-center justify-between">
                <div class="w-full relative flex justify-between lg:w-auto  px-4 lg:static lg:block lg:justify-start">
                <a class="text-sm font-bold leading-relaxed inline-block mr-4 py-2 whitespace-nowrap uppercase text-white" href="#pablo">
                    Shagun Sweets Market Place
                </a>
                <button class="cursor-pointer text-xl leading-none px-3 py-1 border border-solid border-transparent rounded bg-transparent block lg:hidden outline-none focus:outline-none" type="button">
                    <span class="block relative w-6 h-px rounded-sm bg-white"></span>
                    <span class="block relative w-6 h-px rounded-sm bg-white mt-1"></span>
                    <span class="block relative w-6 h-px rounded-sm bg-white mt-1"></span>
                </button>
                </div>
                <div class="lg:flex flex-grow items-center" id="example-navbar-warning">
                <ul class="flex flex-col lg:flex-row list-none ml-auto">
                    <li class="nav-item">
                        <a class="px-3 py-2 flex items-center text-xs uppercase font-bold leading-snug text-white hover:opacity-75" href="#pablo">
                        {{ account }}
                        </a>
                    </li>
                    <li class="nav-item">
                        <a class="px-3 py-2 flex items-center text-xs uppercase font-bold leading-snug text-white hover:opacity-75" href="#pablo">
                        Login
                        </a>
                    </li>
                    <li class="nav-item">
                        <a class="px-3 py-2 flex items-center text-xs uppercase font-bold leading-snug text-white hover:opacity-75" href="#pablo">
                        Signup
                        </a>
                    </li>
                </ul>
                </div>
            </div>
        </nav>
        <div v-if="loading" class="flex justify-center w-full">
            <svg class="animate-spin h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
              <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
              <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
            </svg>
        </div>
        
        <div v-else>
           <h1 class="flex flex-col justify-center items-center">Add Product</h1>
            <div class="flex flex-col justify-center items-center">
                <form @submit.prevent="createProduct" class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
                    <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
                        Product Name: 
                    </label>
                    <input v-model="formData.name" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="username" type="text" placeholder="product name" required>
                    </div>
                    <div class="mb-6">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="password">
                        Product Price:
                    </label>
                    <input v-model="formData.price" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" step="any" id="password" type="number" placeholder="price" required>
                    <!-- <p class="text-red-500 text-xs italic">Please enter the amount.</p> -->
                    </div>
                    <div class="flex items-center justify-between">
                    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="submit">
                        Add Product
                    </button> 
                    </div>
                </form>
            </div>

        <div class="flex flex-col">
         <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
         <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
             <div class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg">
             <table class="min-w-full divide-y divide-gray-200">
                 <thead class="bg-gray-50">
                 <tr>
                     <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                     Name
                     </th>
                     <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                     Price 
                     </th>
                     <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                     Owner
                     </th>
                     <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                     Button
                     </th>
                    
                 </tr>
                 </thead>
                 <tbody class="bg-white divide-y divide-gray-200">
                 <tr v-for="product in products" :key="product.id">
                     <td><div class="text-sm text-gray-900">{{ product.name }}</div></td>
                     <td><div class="text-sm text-gray-900"> {{ product.price }} Wei</div></td>
                     <td><div class="text-sm text-gray-900">{{ product.owner }}</div></td>
                    <td class="px-6 py-4 whitespace-nowrap">
                          <button v-if="!product.purchased" v-on:click="purchaseProduct( product.id , product.price)" class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-100 text-green-800">Buy</button>
                     </td> 
                    
                 </tr>
                 </tbody>
             </table>
             </div>
         </div>
         </div>
     </div>

        </div>
    </div>
</template>






<script>
import Web3 from 'web3'
import Marketplace from '../abis/Marketplace.json'
const people = [
  {
    name: 'Jane Cooper',
    title: 'Regional Paradigm Technician',
    department: 'Optimization',
    role: 'Admin',
    email: 'jane.cooper@example.com',
    image:
      'https://images.unsplash.com/photo-1494790108377-be9c29b29330?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=4&w=256&h=256&q=60',
  }]

export default {
    name: 'Dashboard',
    data(){
        return{
            people,
            marketplace : null,
            name : "Devpal",
            account : "account",
            productCount : 0,
            products : [],
            loading : true,
            formData : {
                name : '',
                price : ''
            }
        }
    },
 created(){
      console.log("Dashboard created")
      this.checkMetaMask()
      this.loadBlockchainData()
  },
  methods:{
      async checkMetaMask(){
          if(window.ethereum){
              window.web3 = new Web3(window.ethereum)
              await window.ethereum.enable()
          }
          else if (window.web3){
              window.web3 = new Web3(window.web3.currentProvider)
          }
          else{
              window.alert('Non-Ethereum browser detected. You should consider trying MetaMask!')
          }
      },
     async loadBlockchainData(){
         const web3 = window.web3
         const accounts = await web3.eth.getAccounts()
         console.log(accounts)
         this.account = accounts[0]
         // Load Smart Contract......
         const networkId = await web3.eth.net.getId()
         const networkData = Marketplace.networks[networkId]
         if(networkData){
             const marketplace = new web3.eth.Contract(Marketplace.abi,networkData.address)
             this.marketplace = marketplace
             this.loading = false
             console.log(marketplace)
             const productCount = await marketplace.methods.productCount().call()
             //Fetch all Products....
             for(var i=1;i<=productCount;i++){
                 const product = await marketplace.methods.products(i).call()
                 this.products.push(product)
             }
             console.log(this.products)
             
         }
         else{
             window.alert("Contract not deployed to detected network")
         }
     },
     async createProduct(event){
         event.preventDefault()
         this.loading = true
         const data = JSON.parse(JSON.stringify(this.formData))
         const name = data.name.toString()
         const price = window.web3.utils.toWei(data.price.toString(),'Ether')

        this.marketplace.methods.createProduct(name,price).send({ from: this.account, gas: 6000000 }).once('receipt',(receipt)=>{
            this.loading = false
        })
     },
     async purchaseProduct(id,price){
        //  this.loading = true
         console.log(id,price)
        this.marketplace.methods.purchaseProduct(id).send({ from: this.account, value:price, gas: 6000000 }).once('receipt',(receipt)=>{
            this.loading = false
        })
     },
     
  }
}
</script>


