<template>
            <div>
            <div class="container-fluid"> 
            <div class="row mt-5"> 
            <div class="col-8 mx-auto mb-5 shadow">
                
            <h5 class="bg-primary rounded mx-5 mt-4 text-white py-2"><strong> <i class="fas fa-list-alt"></i> To Do App</strong></h5>
  
            <div  v-show="success === true" class="alert alert-success mx-4" role="alert">
            A New List Added! <i class="fas fa-thumbs-up"></i>
            </div>

            <div v-show="failed === true" class="alert alert-danger mx-4" role="alert">
             Please Fill to add a new List <i class="fas fa-exclamation"></i>
            </div>


            <div class="input-group my-4 py-3 shadow">
            <input type="text"
            v-model="newList"
            class="form-control  ml-3" 
            placeholder="New List..." 
            aria-label="New List..." 
            aria-describedby="basic-addon2">
            <div class="input-group-append">
            <button class="btn btn-primary mr-3" @click="createNewList"> <strong><i class="fas fa-plus"></i> New List</strong></button>
            </div>
            </div> 
             <p class="bg-primary rounded mx-5 mt-4 text-white py-2"><strong> <i class="fas fa-list-ol"></i> My Lists</strong></p>
    
            <myLists :newListArray="newListArray"/>
           
            </div>

            </div>
           
           <div class="row">
           <div class="col-8 mx-auto bg-primary rounded shadow">
           <p class="text-white font-weight-bold pt-1"> organize yourself <i class="fas fa-columns"></i></p>
           </div>
           </div>

            </div>  
            </div>
</template>
<script>
    //    <<<<< VANILLA JAVASCRIPTS STARTS:(to stop the array from being empty on page reload) >>>>>
            let newListArray
            if (localStorage.myLists) {
            newListArray = JSON.parse(localStorage.myLists);
            } else {
            newListArray = [];
            }
// <<<<< VANILLA JAVASCRIPTS STARTS >>>>>

            import myLists from './myLists'
            export default {
    
            name: 'CreateList',
            components:{
            myLists
            },
            data(){
            return{
            newList : "",
            newListObj:'',
            newListArray: newListArray,
            success: false,
            failed: false
            }
        },
        methods:{
            createNewList(){
                if (this.newList == ''){
                this.failed = true
                setTimeout(()=> this.failed = false, 500) 
                }else{
                this.success = true
                this.newListObj = {listName:this.newList, task: []}
                this.newListArray.push(this.newListObj)
                this.newList = ''
                let myJSON = JSON.stringify(this.newListArray)
                localStorage.setItem('myLists', myJSON) 
                setTimeout(()=> this.success = false, 500) 
                
                }
                
            }
        }
    }
</script>
<style>
    
</style>