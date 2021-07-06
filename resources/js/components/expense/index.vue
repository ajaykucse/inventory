<template>
	<div>
		<div class="row">
			<router-link class="btn btn-primary" to="/store-expense">Add Expense</router-link>
		</div>
		<br>
		<input type="text" v-model="searchTerm" class="form-control" placeholder="Search..." style="width:300px;">
		<br>
	   <div class="row">
            <div class="col-lg-12 mb-4">
              <!-- Simple Tables -->
              <div class="card">
                <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
                  <h6 class="m-0 font-weight-bold text-primary">Expense List</h6>
                </div>
                <div class="table-responsive">
                  <table class="table align-items-center table-flush">
                    <thead class="thead-light">
                      <tr>
                        <th>Details</th>
                        <th>Amount</th>
                        <th>Expense Date</th>
                        <th>Action</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="expense in filtersearch" :key="expense.id">
                        <td>{{ expense.details}}</td>
                        <td>{{ expense.amount}}</td>
                        <td>{{ expense.expense_date}}</td>
                        <td>
                           <a @click="deleteExpense(expense.id)" class="btn btn-danger btn-sm">
                            <font color="FFFFFF"> <i class="fas fa-trash"></i></font>
                          </a>

                          <router-link :to="{name: 'edit-expense', params:{id:expense.id}}" class="btn btn-info btn-sm">
                              <i class="fas fa-edit"></i>
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
    		expenses:[],
    		searchTerm:''
    	}
    },
    computed:{
    	filtersearch(){
    		return this.expenses.filter(expense => {
    			return expense.expense_date.match(this.searchTerm)
    		})
    	}
    },

    methods:{
    	allExpense(){
    		axios.get('/api/expense/')
    		.then(({data}) => (this.expenses = data))
    		.catch(console.log('error'))
    	},
    	deleteExpense(id){
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
			  	axios.delete('/api/expense/'+id)
			  	.then(() =>{
			  	this.expenses = this.expenses.filter(expense =>{
			  		return expense.id != id
			  	})
			  	})
			  	.catch(() => {
			  		this.$router.push({name: 'expense'})
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
    	this.allExpense();
    }
  }
</script>


<style type="text/css">
 
</style>
