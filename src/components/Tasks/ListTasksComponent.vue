<template>
    <div class="container">
        <div class="head"><h1>{{ page_title }}</h1></div>
        <div class="row">
            <div class="col">
                <form class="form form-inline form-new-task" @submit.prevent="onSubmit">
                    <input type="text" placeholder="Nova Tarefa" class="form-control" v-model="task.name">
                    <button type="submit" class="btn btn-success">Salvar</button>
                </form>
            </div>
            <div class="col">
                <form class="form form-inline form-search" @submit.prevent="searchTasks">
                    <input type="text" placeholder="Pesquisar" class="form-control" v-model="filterSearch">
                </form>
            </div>
        </div>
        <hr>
        <table class="table table-striped">
            <thead>
                <tr>
                <th scope="col-1">Id</th>
                <th scope="col-8">Nome</th>
                <th scope="col-3">Ações</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(task, index) in searchTasks" :key="index">
                    <td>{{task.id}}</td>
                    <td>{{task.name}}</td>
                    <td class="">
                        <a href="#" class="btn btn-info" @click.prevent="edit(task.id)">Editar</a>
                        <a href="#" class="btn btn-danger" @click.prevent="deleteTask(task.id)">Excluir</a>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    
</template>

<script>
export default {
    data() {
        return{
            page_title : 'Listagem de Tarefas',
            tasks: [
            ],
            task:{
                id: '',
                name: ''
            },
            updating: false,
            updateIndex : '',
            filterSearch: ''
        }
    },
    methods:{
        onSubmit(){
            if( this.updating ){
                this.update()
                return
            }
                
            this.save()
            
        },
        save (){
            this.task.id = this.tasks.length + 1
            this.tasks.push(this.task)

            this.cleanForm()
        },
        edit(id){
            this.updateIndex = this.findIndexItem(id)
            this.task = this.tasks[this.updateIndex]        

            this.updating = true

        },
        update(){
            this.tasks[this.updateIndex] = this.task

            this.updating = false

            this.cleanForm()
        },
        cleanForm(){
            this.task = {
                id: '',
                name: ''
            }
        },
        deleteTask(id){
            let index = this.findIndexItem(id)
            this.tasks.splice(index , 1)
        },
        findIndexItem(id){
            for(let index = 0; index < this.tasks.length -1; index++){
                if(this.tasks[index].id == id)
                return index
            }
        }
    },
    computed:{
        searchTasks(){
            if (this.filterSearch === '')
                return this.tasks
            
            let vm = this

            return this.tasks.filter(task => {
                return task.name.toLowerCase().indexOf(vm.filterSearch.toLocaleLowerCase()) > -1
            })
        }
    }
}
</script>

<style scoped>
.head {
    margin: 50px 0;
}
.form-new-task {
    display: inline-flex;
    margin-bottom: 20px;
}
.form-new-task input {
    margin-right: 5px;
}
</style>
