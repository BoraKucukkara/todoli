<template>
  <div id="app">

    <input v-model="nameHolder" type="text" class="input-note" @keyup.enter="pushNote()"><a href="#" class="add" :class="pushAnim ? 'input-anim' : ''" @click="pushNote()"><font-awesome-icon icon="fa-solid fa-plus" /></a>

    <div v-if="this.todo.length === 0" class="empty-screen">
      <img src="./assets/todoli.svg"/>
      <p>Add a note</p>
      <span>to complate a task, drag them to green circle down below</span>
    </div>
    <draggable tag="div"
        class="list-container"
        v-model="todo"
        v-bind="dragOptions"
        @start="drag = true"
        @end="drag = false"
        @remove="taskDone">
      <transition-group type="transition" :name="!drag ? 'flip-list' : null">
      <div class="todo" v-for="(task, index) in todo" :style="{'background': task.background}" :key="index">{{task.name}}</div>
      </transition-group>
    </draggable>
    <div v-if="confirmDone" class="taskDone">task done!</div>
    <draggable tag="div"
        class="del" :class="drag ? 'drop-anim' : ''"
        v-model="done"
        v-bind="dragOptions"
        @start="drag = true"
        @end="drag = false">
        <font-awesome-icon icon="fa-solid fa-check" />
        <!--<transition-group type="transition" :name="!drag ? 'flip-list' : null">
        
      <div v-for="(task, index) in done" class="done" :key="index">{{task}}</div>
      </transition-group>-->
    </draggable>
  </div>
</template>

<script>
import draggable from 'vuedraggable'
export default {
  name: 'App',
  components: {
    draggable
  },
  data() {
    return {
      todo: [/*{name:"sdlf",background:""}*/],
      done: [],
      drag: false,
      confirmDone: false,
      nameHolder: "",
      colorHolder: "",
      pushAnim: false
    }
  },
  methods:{
    pushNote(){
      this.randomizeColor();
      this.todo.push({'name': this.nameHolder, 'background': this.colorHolder});
      this.nameHolder = ""
      this.pushAnim = true;
      setTimeout(()=> {
        this.pushAnim = false}, 500);
    },
    randomizeColor() {
      return this.colorHolder = "hsl(" + Math.floor(Math.random() * 360) + ",70%,40%)";
    },
    taskDone(){
      this.confirmDone = true;
      setTimeout(()=> {
        this.confirmDone = false}, 1000);
        this.done = []
    },
    dropAniStart() {
      let drop = document.getElementsByClassName("drop-zone");
      drop.classList.add('drop-anim');
    },
    dropAniStop() {
      let drop = document.getElementsByClassName("drop-zone");
      drop.classList.remove('drop-anim');
    }
    
  },
  computed: {
    dragOptions() {
      return {
        animation: 200,
        group: "description",
        disabled: false,
        ghostClass: "ghost",
        dragClass: "drag-item",
        dropClass: "drop-zone",
      };
    }
  }
  // beforeMount(){this.randomColor()}
}
</script>

<style>
* {list-style:none;box-sizing:border-box;margin:0;}
body {
  background: hsl(259, 33%, 20%);
}
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  width: 100vw;
  height: 100vh;
}
.list-container {
  width:100%;
  min-height:50vh;
  padding:1rem;
}
.todo, .done {
  word-break: break-all;
  border-radius:2rem;
  color:#fff;
  font-size: 1.3rem;
  padding:.5rem 1.2rem .5rem 1.2rem;
  margin: .2rem .2rem;
  line-height: 2rem;
  cursor: move;
  width:max-content;
  line-break: normal;
  display:inline-flex;
  box-shadow: 0px 10px 0px hsla(0, 0%, 0%, 0.1);
  animation: fade-in 0.5s forwards;
}
@keyframes fade-in {
  from {opacity:0}
  to {opacity: 1}
}
.add{
  position:fixed;
  bottom: 1rem;
  right:1rem;
  background:#4f2f5d;
  border-radius:50%;
  width:5rem;
  height: 5rem;
  line-height: 5rem;
  font-size:1.5rem;
  color:#fff;
  }
.add:visited {text-decoration: none;}
.del{
  position:fixed;
  bottom: 1rem;
  left:1rem;
  background:#159d00;
  border-radius:50%;
  width:5rem;
  height: 5rem;
  line-height: 5rem;
  color: #ffffff;
  font-size:1.3rem;
 }
 /* .drop-anim:before {
  position:fixed;
  content: "DRAG HERE";
  font-size: .8rem;
  bottom: 5rem;
  left:1.1rem;
  color:#574a5e;
  animation: fade-in-out 0.5s infinite;
 } */
 .drop-anim{
    animation: dance 0.5s infinite;
  }
  @keyframes dance {
    0% {background: #159d00;}
    50%{background: #65ea00;}
    100%{background: #159d00;}
  }

  input:focus {
    outline: none;
}
  .input-note {
  position:fixed;
  bottom: 1rem;
  right:1rem;
  left:3rem;
  border:5px solid #4f2f5d;
  background: hsl(259, 33%, 20%);
  border-radius:2.5rem;
  margin-left:5rem;
  height: 5rem;
  line-height: 5rem;
  color: #ffffff;
  font-size:1.5rem;
  padding:0 5rem 0 2rem;
  }
  .input-anim {
    animation: inputanim .5s forwards;
  }
@keyframes inputanim {
    0% {transform: rotate(0deg);}
    100%{transform: rotate(360deg);}
  }
  .drag-item {
  border-radius:2rem;
  font-size: 1.2rem;
  padding:.5rem 1.2rem .5rem 1.2rem;
  margin:1rem;
  line-height: 2rem;
  width:max-content;
  box-shadow: 0px 15px 3px hsla(0, 0%, 0%, 0.3);
  }
.empty-screen {
  position:fixed;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  top:0;
  width:100vw;
  height: 50vh;
  color:#574a5e;
}
.empty-screen p {
  font-size: 4em;
}
.empty-screen span {
  font-size: 1.2em;
}
.empty-screen img{
  width: 50vw;
  margin-bottom: 3rem;
  max-width:20rem;

}
.taskDone{
  position:fixed;
  display: flex;
  align-items: center;
  justify-content: center;
  top:0;
  width:100vw;
  height: 100vh;
  font-size: 5rem;
  color:#6d6075;
  z-index:-1;
  animation: fade-in-out 1s forwards;
  }
  @keyframes fade-in-out {
    0% {opacity: 0;}
    30% {opacity: 1;}
    70% {opacity: 1;}
    100% {opacity: 0;}
    
  }

.flip-list {
  transition: transform 0.5s;
}
.no-move {
  transition: transform 0s;
}
.ghost {
  opacity: 0.0;
}
</style>
