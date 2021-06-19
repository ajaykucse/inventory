<template>
	<div>
		<div class="row">
			<router-link class="btn btn-primary" to="/store-supplier">Add Supplier</router-link>
		</div>
		<br>
		<input type="text" v-model="searchTerm" class="form-control" placeholder="Search..." style="width:300px;">
		<br>
	   <div class="row">
            <div class="col-lg-12 mb-4">
              <!-- Simple Tables -->
              <div class="card">
                <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
                  <h6 class="m-0 font-weight-bold text-primary">Supplier List</h6>
                </div>
                <div class="table-responsive">
                  <table class="table align-items-center table-flush">
                    <thead class="thead-light">
                      <tr>
                        <th>Name</th>
                        <th>Photo</th>
                         
                        <th>Phone</th>
                        <th>Shop Name</th>
                        <th>Action</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="supplier in filtersearch" :key="supplier.id">
                        <td>{{ supplier.name}}</td>
                        <td><img :src="supplier.photo" id="sup_photo"></td>
                        
                        <td>{{ supplier.phone}}</td>
                        <td>{{ supplier.shopname}}</td>
                        <td>
                        	<router-link :to="{name: 'edit-supplier', params:{id:supplier.id}}" class="btn btn-sm btn-primary">Edit</router-link>

                        	<a @click="deleteSupplier(supplier.id)" class="btn btn-sm btn-danger"><font color="FFFFFF">Delete</font></a>
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
    		suppliers:[],
    		searchTerm:''
    	}
    },
    computed:{
    	filtersearch(){
    		return this.suppliers.filter(supplier => {
    			return supplier.name.match(this.searchTerm)
    		})
    	}
    },

    methods:{
    	allSupplier(){
    		axios.get('/api/supplier/')
    		.then(({data}) => (this.suppliers = data))
    		.catch(console.log('error'))
    	},
    	deleteSupplier(id){
    		Swal.fire({
			  title: 'Are you sure?',
			  text: "You won't be able to revert this!",
			  icon: 'warning',
			  showCancelButton: true,
			  confirmButtonColor: '#3085d6',
			  cancelButtonColor: '#d33',
			  confirmButtonText: 'Yes, delete it!'
			}).then((result) => {
			  if (result.isConfirmed) {
			  	axios.delete('/api/supplier/'+id)
			  	.then(() =>{
			  	this.suppliers = this.suppliers.filter(supplier =>{
			  		return supplier.id != id
			  	})
			  	})
			  	.catch(() => {
			  		this.$router.push({name: 'supplier'})
			  	})
			    Swal.fire(
			      'Deleted!',
			      'Your file has been deleted.',
			      'success'
			    )
			  }
			})
    	}
    },
    created(){
    	this.allSupplier();
    }
  }
</script>


<style type="text/css">
	#sup_photo{
		height: 40px;
		width: 40px;
	}
</style>
