<template>
  <div>
      <div class="container invoice">
         <div class="row">
           <div class="col-12 text-center ">

             <h3 class="text-center text-primary my-3 text-dark">Invoice Maker</h3>
             <form action=""  class="hide-in-print" @submit.prevent="saveRecord">
               <div class="row">
                 <div class="col-6">
                   <select class="form-select" v-model="selectedService">
                     <option value="" disabled>Select Service</option>
                     <option v-for="service in services" :key="service.id" :value="service.id">
                       {{service.name}}
                     </option>
                   </select>
                 </div>
                 <div class="col-3">
                   <input type="number" v-model="inputQuantity" class="form-control" placeholder="Quantity">
                 </div>
                 <div class="col-3">
                   <button class="btn btn-primary w-100">
                     <i class="fa-solid fa-plus"></i>
                   </button>
                 </div>
               </div>
             </form>
             <table class="table table-bordered mt-3">
               <thead>
                   <tr>
                     <th>#</th>
                     <th>Service</th>
                     <th>Price</th>
                     <th>Quantity</th>
                     <th>Cost</th>
                   </tr>
               </thead>
               <tbody>
                   <tr v-if="records.length === 0">
                     <td colspan="5" class="text-center">No Record Yet</td>
                   </tr>

                   <tr class="animate__animated animate__fadeInDown" v-for="record in records" :key="record.id">
                      <td><span>{{record.id}}</span>
                        <i @click="del(record.id)" class="fas fa-trash-alt text-danger  hide-in-print"></i>
                      </td>
                      <td>{{record.service.name}}</td>
                      <td class="text-center">{{record.service.price}}</td>
                      <td class="text-center">{{record.quantity}}</td>
                      <td class="text-center">{{record.cost}}</td>
                   </tr>
               </tbody>
               <tfoot>
                   <tr v-if="records.length > 0">
                     <td colspan="4" class="text-center">Total Cost</td>
                     <td class="text-center">
                       <span>{{totalCost}}$</span>
                     </td>
                   </tr>
               </tfoot>
             </table>
             <div class="row mt-3 g-2 hide-in-print pb-3">
                  <div class="col-6">
                    <input v-model="invoiceNumber" class="form-control">
                  </div>
                  <div class="col-3">
                       <button @click="toPrint()" class="btn btn-secondary w-100">
                         <i class="fa-solid fa-print"></i>
                         Print
                       </button>
                  </div>
                  <div class="col-3">
                 <button @click="toPrint()" class="btn btn-primary w-100">
                   <i class="fa-solid fa-file"></i>
                   Save
                 </button>
                 </div>
             </div>
           </div>

         </div>
      </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedService:'',
      inputQuantity:'',
      services:[
         {
           id:1,
           name:'Web Design($100)',
           price:'100'
         },
         {
           id:2,
           name:'Domain Service($10)',
           price:'10'
         },
         {
           id:3,
           name:'SSL($5)',
           price:'5'
         },
         {
           id:4,
           name:'Maintenance($50)',
           price:'50'
         }

       ],
      recordStart :1,
      records:[],
      invoiceId:'',
    }
  },
  computed:{
    totalCost(){
      return this.records.reduce((p,c)=>p+c.cost,0);
    },
    invoiceNumber(){
           let d = new Date();
           let dateCode = d.getDate()+"-"+(d.getMonth()+1)+"-"+d.getFullYear();
           let random=Math.floor(Math.random()*10000);
           return dateCode+''+random;
    }
  },
  methods: {
       saveRecord(){
        let currentService = this.services.find(service=>service.id === this.selectedService);
        let cost = currentService.price * this.inputQuantity;
        let record = {
          id:this.recordStart,
          service:currentService,
          quantity:this.inputQuantity,
          cost
        }
        this.records.push(record);
        // console.log(record);
        this.recordStart++;

       },
       toPrint(){
         print();
       },
       del(recordId){
         console.log(recordId);
         this.records = this.records.filter(record=>record.id != recordId);
       }

  },
}
</script>

<style lang="scss">
$secondary:#35495e;
@import "~bootstrap/dist/css/bootstrap.min.css";
@import "~@fortawesome/fontawesome-free/css/all.min.css";
@import "~animate.css/animate.min.css";

.invoice{
  background: #42b983;
}

@media print {
  .hide-in-print{
    display: none;
  }
}
.logo-img{
  height: 150px;
}

</style>