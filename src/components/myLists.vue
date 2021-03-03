<template>
        <div>
            
        <h5 v-for="(item, index) in  newListArray"
        :key="index" class="text-left ml-2 pb-2 pt-3"> {{item.task.length}} <i class="fas fa-trash-alt text-danger" @click="deleteList(item, index)"></i> <i class="fas fa-list-alt text-primary" @click="listDetails(item.listName)"></i> {{item.listName}}  </h5>
        
        </div>
        
</template>
<script>
export default {
    name:'myLists',
    props:{
    newListArray: Array
    },
    

    methods:{
    listDetails(value){
    this.$router.push({name:'Details', params:{id:value}})
         
       },
    deleteList(index) {
    this.newListArray.splice(index,1)
    let myJSON = JSON.stringify(this.newListArray)
    localStorage.setItem('myLists', myJSON) 
    }
    }
}
</script>
<style>
    
</style>