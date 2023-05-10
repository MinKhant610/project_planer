<template>
    <div class="project" :class="{complete:project.complete}">
        <div class="flexing">
            <div >
                <h3 @click="clickProject">{{ project.title }}</h3>
            </div>
            <div>
                <i class="fa-solid fa-trash-can" @click="delProject"></i>
                <router-link :to="{name:'editproject', params: {id:project.id}}">
                    <i class="fa-solid fa-pen"></i>
                </router-link>
                <i class="fa-solid fa-check fa-lg" @click="completeProject"></i>
            </div>
        </div>
        <p v-if="show_detail"> {{ project.detail }}</p>
    </div>
</template>

<script>

    export default {
        props : ['project'],
        data(){
            return {
                show_detail : false,
                api : 'http://localhost:3000/projects/',
            }
        },
        methods :{
            clickProject(){
                this.show_detail = !this.show_detail;
            },
            delProject(){
                // there is no method , defalut be GET method
                let delete_route = this.api + this.project.id;
                fetch(delete_route, {method : "DELETE"})
                .then(()=>{
                    this.$emit("delete", this.project.id);
                })
                .catch((err)=>{
                    console.log(err)
                })
            },
            completeProject(){
                // to update project => for done icon
                let update_complete_route = this.api + this.project.id;
                fetch(update_complete_route, {
                    method : "PATCH",
                    headers: {
                        // application/json mean => it json format
                        "Content-Type" : "application/json" 
                    },
                    body : JSON.stringify({
                        // change complete project to not complete in database and also not complet to complete
                        complete : !this.project.complete
                    })
                })
                .then(()=>{
                    this.$emit("complete", this.project.id);
                })
                .catch((err)=>{
                    console.log(err);
                })
            }
        }
    }
</script>

<style scoped>
.project {
    padding: 20px;
    margin: 10px;
    background: #f2f2f2;
    border-left: 5px solid red;
    border-radius: 8px;
}
h3{
    color: black;
    cursor: pointer;
}

/* flex box for title and font */
.flexing {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

i{
    margin-left: 12px;
    cursor: pointer;
}
i:hover.fa-trash-can{
    color: red;
}
i:hover.fa-pen{
    color:blue;
}
i:hover.fa-check{
    color: rgb(20, 218, 20);
}
.complete{
    border-left-color: #22eaaa;
}
</style>