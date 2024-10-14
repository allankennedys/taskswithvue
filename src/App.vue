<template>
  <div>
    <h1>Minha Lista de Tarefas</h1>
    <!--Abaixo é criado uma função dentro do botão para que quando o usuário clique nele, 
    o showList se torne positivo, exibindo a lista ou vice-versa -->
    <button @click="showHideList()">Mostrar/Ocultar Lista</button>
    <br>
    <input v-focus type="text" v-model="currentTask" @keyup.enter="addTask">
    <button @click="addTask">Adicionar Tarefa</button>

    <!--Abaixo inserimos um v-if na lista para que, por padrão, a lista não seja exibida a menos 
    que o usuário pressione o botão acima. Isso acontece porque showList no nosso data é declarado como false-->
    <ul v-if="showList">
      <li  v-for="(task, index) in tasks"
      @dblclick="complete(task)"
        :key="`${task}-${index}`"
        class="task-item"
        :class="{
          'sublinhado': task.isDone
        }">
        
        {{ task.name }}
        <button @click="remove(task)">&times;</button>

      </li>
    </ul>
    <!--Em condição oposta ao v-if é exibido o parágrafo abaixo informando que a lista está oculta-->
    <p v-else>Lista de Tarefas Oculta</p>
  </div>
</template>

<script>
const focus = {
  inserted (element){
    element.focus()

  }
}
export default {
  directives:{
    focus
  },
data: () => ({
  currentTask: '',
  showList: false,
  tasks: [
    {name: 'fazer o curso', isDone: false, idade:'8 anos'}
  ],
  
}),
methods:{
  remove(task){
    this.tasks = this.tasks.filter(t => t.name !== task.name)

  },
  showHideList(){
   this.showList = !this.showList
  },
  complete(tasks){
    this.tasks = this.tasks.map(t =>{
      if(t.name === tasks.name){
        return{...t,isDone: !t.isDone}
      }
      return{...t}
    })
  },
  addTask(){
    if(this.currentTask.trim()){
      this.tasks.push({
      name: this.currentTask,
      isDone: false
    })
    }
    
    this.currentTask = ''
  }
  
}
}
</script>

<style>

body{
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}
h1{
  font-size: 30pt;
}
li{
  padding: 6px;
}
input{
  margin: 15px;
}
button{
  background-color: rgb(59, 77, 77);
  color: white;
}
.sublinhado{
  text-decoration: line-through;
}
.task-item{
    cursor: pointer;
  }
</style>
