<template>
    <div>
        <div class="container-fluid"> 
        <div class="row mt-5"> 
        <div class="col-8 mx-auto mb-5 shadow">
                    
        <h5 class="bg-primary rounded mx-5 mt-4 text-white py-2"><strong><i class="fas fa-tasks"></i> Tasks</strong></h5>
            
        <div v-for="(item,index) in myJSON" :key="index" v-show="proId == item.listName" class="text-left">



        <h5 class="text-left ml-2 pt-2 mb-3"><i class="fas fa-list-ul"></i> {{item.listName.toUpperCase()}} ({{item.task.length}})</h5>
         <div v-show="failed === true" class="alert alert-danger" role="alert">
          Please Fill to edit task name <i class="fas fa-exclamation"></i>
            </div>
        
        <div v-for="(n,index) in item.task" :key="index" class="border-0 rounded mb-4 mr-5 ml-2 back-ground">
  
       <input type="checkbox" class="form-check-input ml-1" @click="doneTask(n.increase, n)" v-model="n.increase">
       
        
       
       <h5 class="text-left ml-4 pt-1 mr-5 d-inline" :class="{completed : n.increase}" > {{n.taskName}} </h5>
    
       <div class="form-check">
       <input class="form-check-input mr-3" type="checkbox" value="" id="defaultCheck1" @click="editTask(n.checked,n.taskName)" v-model="n.checked">
       <i class="fas fa-edit"></i>
       <label class="form-check-label text-primary font-sm" for="defaultCheck1">
        Edit 
       </label>
    
       </div> 
       <p class="font-sm text-primary ml-2 font-weight-bold">Date Added: {{n.date}}</p>

       <i class="fas fa-trash-alt text-danger pl-1" @click="deleteTask(index, item.task)"></i>
       <ul class="list-group list-group-flush my-4 p-2 text-left" v-show="n.checked">
       <li class="list-group-item">
       <input type="text" class="form-control" 
       @keyup.enter="enterClicked(n.taskName, item.task, index, n.increase)" 
       v-model="name"
       placeholder="Edit Task name...">
       </li>   
       <li class="list-group-item"><i class="fas fa-bell"></i> Remind Me</li>
       <li class="list-group-item"><i class="fas fa-redo-alt"></i> Repeat</li>
            </ul>
          
        </div>
            
           
        <p class="text-center mt-3 text-primary font-weight-bold"> {{getCount}} of {{item.task.length}} Tasks completed <i class="fas fa-clipboard-check"></i> </p>

        <div class="input-group  mt-5 py-3 mb-4 shadow">
        <input type="text" v-model="newTask" class="form-control  ml-3" placeholder="New Task..." >
        <div class="input-group-append">
        <button class="btn btn-primary mr-3" @click="createNewTask(item.task, item.completed)"> <strong><i class="fas fa-plus"></i> Add a Task</strong></button>
        </div>
        </div>
       
        </div>
    
        </div>

        </div>
        <div class="row">
        <div class="col-8 mx-auto bg-primary rounded shadow">
        <p class="text-white font-weight-bold pt-1"> start adding new task <i class="fas fa-plus-square"></i></p>
        </div>
        </div> 

        </div>  
        </div>
</template>
<script>
export default {
    name:'listDetails',
    data(){
        return{
         proId:this.$route.params.id,
         myJSON: '',
         newTask:'',
         newTaskObj:'',
         i: 1,
         name:'',
         showDate: false,
         failed : false

        
        
         
        }
    },

    
    

    methods:{
      createNewTask(taskArray){
        this.newTaskObj = {taskName: this.newTask, increase:false, checked:false, date:new Date().toLocaleString()}
        taskArray.push(this.newTaskObj)
        this.newTask = ''
        let newArr = JSON.stringify(this.myJSON)
        localStorage.setItem('myLists', newArr)   
        },

      doneTask(value){
          
          if (value ){
    
            value == false
            this.$store.commit({type:'decreaseCount', number:this.i})
            
          } else{
            value == true
            this.$store.commit({type:'increaseCount', number:this.i}) 
            this.beep()
          }
        
           

        },

      beep() {
        var snd = new Audio("data:audio/wav;base64,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qJrx4KOQjahZxWbcZgztj2c49nKmkId44S71j0c8eV9yDK6uPRzx5X18eDvjvQ6yKo9ZSS6l//8elePK/Lf//IInrOF/FvDoADYAGBMGb7FtErm5MXMlmPAJQVgWta7Zx2go+8xJ0UiCb8LHHdftWyLJE0QIAIsI+UbXu67dZMjmgDGCGl1H+vpF4NSDckSIkk7Vd+sxEhBQMRU8j/12UIRhzSaUdQ+rQU5kGeFxm+hb1oh6pWWmv3uvmReDl0UnvtapVaIzo1jZbf/pD6ElLqSX+rUmOQNpJFa/r+sa4e/pBlAABoAAAAA3CUgShLdGIxsY7AUABPRrgCABdDuQ5GC7DqPQCgbbJUAoRSUj+NIEig0YfyWUho1VBBBA//uQZB4ABZx5zfMakeAAAAmwAAAAF5F3P0w9GtAAACfAAAAAwLhMDmAYWMgVEG1U0FIGCBgXBXAtfMH10000EEEEEECUBYln03TTTdNBDZopopYvrTTdNa325mImNg3TTPV9q3pmY0xoO6bv3r00y+IDGid/9aaaZTGMuj9mpu9Mpio1dXrr5HERTZSmqU36A3CumzN/9Robv/Xx4v9ijkSRSNLQhAWumap82WRSBUqXStV/YcS+XVLnSS+WLDroqArFkMEsAS+eWmrUzrO0oEmE40RlMZ5+ODIkAyKAGUwZ3mVKmcamcJnMW26MRPgUw6j+LkhyHGVGYjSUUKNpuJUQoOIAyDvEyG8S5yfK6dhZc0Tx1KI/gviKL6qvvFs1+bWtaz58uUNnryq6kt5RzOCkPWlVqVX2a/EEBUdU1KrXLf40GoiiFXK///qpoiDXrOgqDR38JB0bw7SoL+ZB9o1RCkQjQ2CBYZKd/+VJxZRRZlqSkKiws0WFxUyCwsKiMy7hUVFhIaCrNQsKkTIsLivwKKigsj8XYlwt/WKi2N4d//uQRCSAAjURNIHpMZBGYiaQPSYyAAABLAAAAAAAACWAAAAApUF/Mg+0aohSIRobBAsMlO//Kk4soosy1JSFRYWaLC4qZBYWFRGZdwqKiwkNBVmoWFSJkWFxX4FFRQWR+LsS4W/rFRb/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////VEFHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAU291bmRib3kuZGUAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMjAwNGh0dHA6Ly93d3cuc291bmRib3kuZGUAAAAAAAAAACU=");  
        snd.play();
        },



      deleteTask(index, task){
          task.splice(index,1) 
          let newArr = JSON.stringify(this.myJSON)
          localStorage.setItem('myLists', newArr)  
        },

  

      editTask(value,task){
        this.name = task
        if (value == false){
           value = false
        }else{
          value = true
           
        }
          
        },
       

      enterClicked(value, item, index){
        if(this.name == ""){
          this.failed = true
           setTimeout(()=> this.failed= false, 1000) 
        }
        else if(value){
        value = this.name 
        item[index].taskName = value
        this.name =''
        let newArr = JSON.stringify(this.myJSON)
        localStorage.setItem('myLists', newArr) 
        }
      
       
        


        
      },
      addDate(){
      this.showDate = !this.showDate
        
      },




     
    },
        
    

     computed:{
        getCount(){
            return this.$store.state.count
            
        }
    },

     created(){
       this.myJSON = JSON.parse(localStorage.getItem('myLists'))
    },
    
}
</script>
<style scoped>
  .back-ground{
      background-color: #F9F9F9;
  }

   .completed {
  text-decoration: line-through;
  color:red
}

  .notcompleted {
  color:#4fc08d;
  font-weight: bold
}

.font-sm{
  font-size: 13px;

}

</style>