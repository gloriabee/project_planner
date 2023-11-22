<template>
  <div class="project" :class="{complete:project.complete}">
    <div class="flexing">
        <div>
            <h3 @click="showDetail = !showDetail">{{ project.title }}</h3>
        </div>

        <div>
            
            <span class="material-icons" @click="deleteProject()"> delete </span>
            <span class="material-icons">edit </span>
            <span class="material-icons" @click="completeProject"> check </span>
        </div>
    </div> 
        <p v-if="showDetail">{{ project.detail }}</p>
        {{ project.complete }}
      
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showDetail: false,
      api:'http://localhost:3000/projects/'
    };
  },
  methods:{
    deleteProject(){
      // fetch(this.api+"/"+this.project.id,{method:"DELETE"})
      let deleteRoute=this.api+this.project.id;
      fetch(deleteRoute,{method:"DELETE"})
      .then(()=>{
        this.$emit("delete",this.project.id)
      })
      .catch((err)=>{
        console.log(err);
      })
    },
    completeProject(){
      let updateCompleteRoute=this.api+this.project.id;
      fetch(updateCompleteRoute,{
        method:"PATCH",
      headers:{
        "Content-Type":"application/json"
      },
      body:JSON.stringify(
        {
          complete:!this.project.complete
        }
      )
      })
      .then(()=>{
        this.$emit("complete",this.project.id);
      })
      .catch((err)=>{
        console.log(err);
      })
    }
  }
};
</script>

<style>
.project {
  padding: 10px 20px;
  background: rgb(226, 222, 222);
  border-left: 5px solid crimson;
  margin: 10px;
  border-radius: 8px;
}
h3 {
  color: indigo;cursor: pointer;
}
.flexing{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
span{
    color: coral;
    margin-left:5px
}
span:hover{
    color: indigo;
    cursor: pointer;
}
.complete{
  border-left-color: green;
}
</style>
