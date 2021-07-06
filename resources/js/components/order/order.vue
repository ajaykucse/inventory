<template>
	<div>
		<br>
		<input type="text" v-model="searchTerm" class="form-control" placeholder="Search..." style="width:300px;">
		<br>
	   <div class="row">
            <div class="col-lg-12 mb-4">
              <!-- Simple Tables -->
              <div class="card">
                <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
                  <h6 class="m-0 font-weight-bold text-primary">Today Orders</h6>
                </div>
                <div class="table-responsive">
                  <table class="table align-items-center table-flush">
                    <thead class="thead-light">
                      <tr>
                        <th>Name</th>
                        <th>Total Amount</th>
                        <th>Pay</th>
                        <th>Due</th>
                        <th>PayBy</th>
                        <th>Action</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="order in filtersearch" :key="order.id">
                        <td>{{ order.name}}</td>
                        <td>{{ order.total}} $</td>
                        <td>{{ order.pay}} $</td>
                        <td>{{ order.due}} $</td>
                        <td>{{ order.payby}}</td>
                        <td>
                            
                        <router-link :to="{name: 'view-order', params:{id:order.id}}" class="btn btn-info btn-sm">
                              <i class="fas fa-eye"></i>
                        </router-link>
                           
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
                <div class="card-footer"></div>
              </div>
            </div>
          </div>
          <!--Row-->
	
    </div>
</template>

<script type="text/javascript">
  export default {
    created(){
      if(!User.loggedIn()){
        this.$router.push({name: '/'})
      }
    },
    data(){
    	return{
    		orders:[],
    		searchTerm:''
    	}
    },
    computed:{
    	filtersearch(){
    		return this.orders.filter(order => {
    			return order.name.match(this.searchTerm)
    		})
    	} 
    },

    methods:{
    	allOrder(){
    		axios.get('/api/orders/')
    		.then(({data}) => (this.orders = data))
    		.catch(console.log('error'))
    	},
    },
    created(){
    	this.allOrder();
    }
  }
</script>


<style type="text/css">
 
</style>
