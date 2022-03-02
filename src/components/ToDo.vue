<template>
  <section class="main">
    <div class="tittle">
      <h1>DOING TASKS</h1>
    </div>
    <div class="form">
      <form @submit.prevent="addTask">

        <input 
        v-model="newTask" 
        name="NewTask" 
        placeholder="New Task">

        <button 
        class="buttonClass" >
          Add
        </button> 

      </form>

      <div class="btnsAll">

        <button 
        @click="remove" 
        class="feedback-button redbtn">
          Remove
        </button>

        <button 
        @click="check" 
        class="feedback-button bluebtn" >
          Check All
        </button>

      </div>
    </div>
    <div class="tasks">
      <section 
      class="rowTable" 
      v-for="(toDo, index) 
      in allTasks" :key="toDo.id">

        <div class="contentTask">

          <input 
          type="checkbox" 
          v-model="toDo.done">

          <p 
          :class="{done: toDo.done}"  
          @click="addText(toDo)">
          {{toDo.content}}
          </p> 

        </div> 

        <div>
          <button 
          title="Edit your task" 
          @click="editTask(index)">

            <svg 
            xmlns="http://www.w3.org/2000/svg" 
            viewBox="0 0 24 24" 
            width="16" 
            height="16">
            <path 
            fill="none" 
            d="M0 0h24v24H0z"/>
            <path 
            d="M6.414 16L16.556 5.858l-1.414-1.414L5 14.586V16h1.414zm.829 2H3v-4.243L14.435 2.322a1 1 0 0 1 1.414 0l2.829 2.829a1 1 0 0 1 0 1.414L7.243 18zM3 20h18v2H3v-2z" 
            fill="rgba(0,0,0,1)"/>
            </svg>

          </button>

          <button 
          title="Delete your task" 
          @click="deleteTask(index)">

          <svg 
          xmlns="http://www.w3.org/2000/svg" 
          viewBox="0 0 24 24" 
          width="16" 
          height="16">
          <path fill="none" 
          d="M0 0h24v24H0z"/>
          <path 
          d="M12 10.586l4.95-4.95 1.414 1.414-4.95 4.95 4.95 4.95-1.414 1.414-4.95-4.95-4.95 4.95-1.414-1.414 4.95-4.95-4.95-4.95L7.05 5.636z" 
          fill="rgba(246,54,54,1)"/>
          </svg>

          </button> 
        </div>      
      </section>
      <h5 
      v-if="allTasks.length === 0">
        There are no tasks to be done
      </h5>
    </div>
    <footer class="footer">
      <p>&copy; {{new Date().getFullYear()}} </p>
    </footer>
  </section>
</template> 

<script>
import {ref} from 'vue';

export default {
  setup(){
    const newTask = ref('');
    const allTasks = ref([]);
    const editIndex = ref(-1);

    //  Add Task
    function addTask() {
      if(newTask.value){
        if(editIndex.value == -1){
          allTasks.value.push({
          id: Date.now(),
          done: false,
          content: newTask.value,      
          })
        } else {  	
          allTasks.value[editIndex.value] = {
            done: allTasks.value[editIndex.value].done,
            content: newTask.value
          }
        }
          editIndex.value = -1;
          newTask.value = '';
      }  
    }

    // Add text content
    function addText(toDo) {
      toDo.done = !toDo.done // buleanos (true or false, of done)
    }

    // Delete a Task
    function deleteTask(index) {
      allTasks.value.splice(index,1);
    }

    // Edit a Task
    function editTask(index) {
      editIndex.value = index;
      newTask.value = allTasks.value[index].content; 
    }

    // Delete all tasks
    function remove() {
      allTasks.value = [];
    }

    // Check Tasks
    function check(){
      allTasks.value.forEach((toDo) => toDo.done = true)
    }
    
    return{
      newTask,
      allTasks,
      addText,
      addTask,
      editTask,
      deleteTask,
      remove,
      check,
    }
  }
}
</script>

<style>
* {
  margin: 0;
}

.main {  
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 10vh 20vh 60vh 10vh;
  gap: 0px 0px;
  grid-auto-flow: row;
  justify-items: center;
  align-items: center;
  justify-content: center;
  grid-template-areas:
    "tittle"
    "form"
    "tasks"
    "footer";
  background: #0F2027;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #2C5364, #203A43, #0F2027);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #2C5364, #203A43, #0F2027); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}

.tittle { grid-area: tittle; }

.form { 
  grid-area: form; 
  display: flex;
  justify-content: space-between;
  width: 80%; 
}

form input {
  height: 100%;
  padding: 0 10px;
  border-radius: 5px;
  margin: 0 5px;
}

form button {
  height: 100%;
  padding: 0 10px;
  border-radius: 5px;
  margin: 0 5px;
  background: transparent;
  color: beige;
}

form button:hover {
  transform: scale(1.1);
}

.tasks { 
  grid-area: tasks; 
  overflow-x: overlay;
  overflow-y: overlay;
  height: -webkit-fill-available;
  width: 80%;
}

.footer { 
  grid-area: footer;
  color: beige;
}

.rowTable {
  display: flex;
  /* width: 500px; */
  justify-content: space-between;
  align-items: center;
  margin: 1%;
  background: hsl(240deg 7% 97%);
  padding: 1%;
  border-top: 1px solid gray;
  border-bottom: 1px solid gray;
  border-radius: 3px;
}
.contentTask {
  display: flex;
  align-items: center;
  gap: 20%;
}

.tasks button {
  border: none;
  background: none;
  cursor: pointer;
}

.tasks button:hover {
  transform: scale(1.5);
}

h1 {
  color: beige;
  font-size: x-large;
  text-align: center;
  border-radius: 20px;
}
h5 {
  font-size: large;
  color: #bebe65;
  text-align: center;
}

.done {
  text-decoration: line-through;
}

.tasks::-webkit-scrollbar {
  width: 5px;
  height: 5px;
}

.tasks::-webkit-scrollbar-thumb {
  background: gray;
  border-radius: 10px;
}

.tasks::-webkit-scrollbar {
  background: rgba(128, 128, 128, 0);
}

.bluebtn {
  border-radius: 5px;
  background: -webkit-linear-gradient(-90deg, #83c3ff 0, #2f81c5 100%);
  background: -moz-linear-gradient(180deg, #83c3ff 0, #2f81c5 100%);
  background: linear-gradient(180deg, #83c3ff 0, #2f81c5 100%);
  -webkit-box-shadow: 0 10px 0 0 rgb(11, 94, 143) ;
  box-shadow: 0 10px 0 0 rgb(11, 94, 143) ;
  text-shadow: 0 0 0 rgb(78, 169, 196) , 1px 1px 0 rgb(78, 169, 196) , 2px 2px 0 rgb(78, 169, 196) , 3px 3px 0 rgb(78, 169, 196) , 4px 4px 0 rgb(78, 169, 196) , 5px 5px 0 rgb(78, 169, 196) , 6px 6px 0 rgb(78, 169, 196) , 7px 7px 0 rgb(78, 169, 196) , 8px 8px 0 rgb(78, 169, 196) , 9px 9px 0 rgb(78, 169, 196) , 10px 10px 0 rgb(78, 169, 196) , 11px 11px 0 rgb(78, 169, 196) , 12px 12px 0 rgb(78, 169, 196) , 13px 13px 0 rgb(78, 169, 196) , 14px 14px 0 rgb(78, 169, 196) , 15px 15px 0 rgb(78, 169, 196) , 16px 16px 0 rgb(78, 169, 196) , 17px 17px 0 rgb(78, 169, 196) , 18px 18px 0 rgb(78, 169, 196) , 19px 19px 0 rgb(78, 169, 196) , 20px 20px 0 rgb(78, 169, 196) , 21px 21px 0 rgb(78, 169, 196) , 22px 22px 0 rgb(78, 169, 196) , 23px 23px 0 rgb(78, 169, 196) , 24px 24px 0 rgb(78, 169, 196) , 25px 25px 0 rgb(78, 169, 196) , 26px 26px 0 rgb(78, 169, 196) , 27px 27px 0 rgb(78, 169, 196) , 28px 28px 0 rgb(78, 169, 196) , 29px 29px 0 rgb(78, 169, 196) , 30px 30px 0 rgb(78, 169, 196) , 31px 31px 0 rgb(78, 169, 196) , 32px 32px 0 rgb(78, 169, 196) , 33px 33px 0 rgb(78, 169, 196) , 34px 34px 0 rgb(78, 169, 196) , 35px 35px 0 rgb(78, 169, 196) , 36px 36px 0 rgb(78, 169, 196) , 37px 37px 0 rgb(78, 169, 196) , 38px 38px 0 rgb(78, 169, 196) , 39px 39px 0 rgb(78, 169, 196) , 40px 40px 0 rgb(78, 169, 196) , 41px 41px 0 rgb(78, 169, 196) , 42px 42px 0 rgb(78, 169, 196) , 43px 43px 0 rgb(78, 169, 196) , 44px 44px 0 rgb(78, 169, 196) , 45px 45px 0 rgb(78, 169, 196) , 46px 46px 0 rgb(78, 169, 196) , 47px 47px 0 rgb(78, 169, 196) , 48px 48px 0 rgb(78, 169, 196) , 1px 1px 0 rgba(196,80,78,0.980392) , 2px 2px 0 rgba(78, 172, 196, 0.961) , 3px 3px 0 rgba(78, 172, 196, 0.941) , 4px 4px 0 rgba(78, 153, 196, 0.922) , 5px 5px 0 rgba(78, 172, 196, 0.902) , 6px 6px 0 rgba(78, 172, 196, 0.882) , 7px 7px 0 rgba(78, 182, 196, 0.863) , 8px 8px 0 rgba(78, 178, 196, 0.843) , 9px 9px 0 rgba(78, 149, 196, 0.82) , 10px 10px 0 rgba(78, 178, 196, 0.8) , 11px 11px 0 rgba(78, 172, 196, 0.78) , 12px 12px 0 rgba(57, 137, 190, 0.761) , 13px 13px 0 rgba(50, 124, 146, 0.741) , 14px 14px 0 rgba(46, 123, 143, 0.722) , 15px 15px 0 rgba(65, 132, 177, 0.702) , 16px 16px 0 rgba(53, 126, 175, 0.682) , 17px 17px 0 rgba(196,80,78,0.658824) , 18px 18px 0 rgba(196,80,78,0.639216) , 19px 19px 0 rgba(196,80,78,0.619608) , 20px 20px 0 rgba(196,80,78,0.6) , 21px 21px 0 rgba(196,80,78,0.580392) , 22px 22px 0 rgba(196,80,78,0.560784) , 23px 23px 0 rgba(196,80,78,0.541176) , 24px 24px 0 rgba(196,80,78,0.521569) , 25px 25px 0 rgba(196,80,78,0.498039) , 26px 26px 0 rgba(196,80,78,0.478431) , 27px 27px 0 rgba(196,80,78,0.458824) , 28px 28px 0 rgba(196,80,78,0.439216) , 29px 29px 0 rgba(196,80,78,0.419608) , 30px 30px 0 rgba(196,80,78,0.4) , 31px 31px 0 rgba(196,80,78,0.380392) , 32px 32px 0 rgba(196,80,78,0.360784) , 33px 33px 0 rgba(31, 75, 95, 0.341) , 34px 34px 0 rgba(25, 81, 114, 0.318) , 35px 35px 0 rgba(48, 98, 139, 0.298) , 36px 36px 0 rgba(196,80,78,0.278431) , 37px 37px 0 rgba(23, 90, 102, 0.259) , 38px 38px 0 rgba(25, 66, 78, 0.239) , 39px 39px 0 rgba(34, 74, 97, 0.22) , 40px 40px 0 rgba(196,80,78,0.2) , 41px 41px 0 rgba(196,80,78,0.180392) , 42px 42px 0 rgba(196,80,78,0.156863) , 43px 43px 0 rgba(196,80,78,0.137255) , 44px 44px 0 rgba(196,80,78,0.117647) , 45px 45px 0 rgba(196,80,78,0.0980392) , 46px 46px 0 rgba(196,80,78,0.0784314) , 47px 47px 0 rgba(196,80,78,0.0588235) , 48px 48px 0 rgba(196,80,78,0.0392157) , 50px 50px 0 rgba(196,80,78,0) ;
}

.bluebtn:hover {
  background: -webkit-linear-gradient(-90deg, #74afd1 0, #55c3d1 100%);
  background: -moz-linear-gradient(180deg, #74afd1 0, #55c3d1 100%);
  background: linear-gradient(180deg, #74afd1 0, #55c3d1 100%);
  transform: scale(1.1);
}

.bluebtn:active {
  background: -webkit-linear-gradient(-90deg, #83c3ff 0, #2f81c5 100%);
  background: -moz-linear-gradient(180deg, #83c3ff 0, #2f81c5 100%);
  background: linear-gradient(180deg, #83c3ff 0, #2f81c5 100%);
  -webkit-box-shadow: 0 5px 0 0 rgb(11, 94, 143) ;
  box-shadow: 0 5px 0 0 rgb(11, 94, 143) ;
}
.redbtn {
  border-radius: 5px;
  background: -webkit-linear-gradient(-90deg, #ff8583 0, #ff5350 100%);
  background: -moz-linear-gradient(180deg, #ff8583 0, #ff5350 100%);
  background: linear-gradient(180deg, #ff8583 0, #ff5350 100%);
  -webkit-box-shadow: 0 10px 0 0 rgba(178,49,49,1) ;
  box-shadow: 0 10px 0 0 rgba(178,49,49,1) ;
  text-shadow: 0 0 0 rgb(196,80,78) , 1px 1px 0 rgb(196,80,78) , 2px 2px 0 rgb(196,80,78) , 3px 3px 0 rgb(196,80,78) , 4px 4px 0 rgb(196,80,78) , 5px 5px 0 rgb(196,80,78) , 6px 6px 0 rgb(196,80,78) , 7px 7px 0 rgb(196,80,78) , 8px 8px 0 rgb(196,80,78) , 9px 9px 0 rgb(196,80,78) , 10px 10px 0 rgb(196,80,78) , 11px 11px 0 rgb(196,80,78) , 12px 12px 0 rgb(196,80,78) , 13px 13px 0 rgb(196,80,78) , 14px 14px 0 rgb(196,80,78) , 15px 15px 0 rgb(196,80,78) , 16px 16px 0 rgb(196,80,78) , 17px 17px 0 rgb(196,80,78) , 18px 18px 0 rgb(196,80,78) , 19px 19px 0 rgb(196,80,78) , 20px 20px 0 rgb(196,80,78) , 21px 21px 0 rgb(196,80,78) , 22px 22px 0 rgb(196,80,78) , 23px 23px 0 rgb(196,80,78) , 24px 24px 0 rgb(196,80,78) , 25px 25px 0 rgb(196,80,78) , 26px 26px 0 rgb(196,80,78) , 27px 27px 0 rgb(196,80,78) , 28px 28px 0 rgb(196,80,78) , 29px 29px 0 rgb(196,80,78) , 30px 30px 0 rgb(196,80,78) , 31px 31px 0 rgb(196,80,78) , 32px 32px 0 rgb(196,80,78) , 33px 33px 0 rgb(196,80,78) , 34px 34px 0 rgb(196,80,78) , 35px 35px 0 rgb(196,80,78) , 36px 36px 0 rgb(196,80,78) , 37px 37px 0 rgb(196,80,78) , 38px 38px 0 rgb(196,80,78) , 39px 39px 0 rgb(196,80,78) , 40px 40px 0 rgb(196,80,78) , 41px 41px 0 rgb(196,80,78) , 42px 42px 0 rgb(196,80,78) , 43px 43px 0 rgb(196,80,78) , 44px 44px 0 rgb(196,80,78) , 45px 45px 0 rgb(196,80,78) , 46px 46px 0 rgb(196,80,78) , 47px 47px 0 rgb(196,80,78) , 48px 48px 0 rgb(196,80,78) , 1px 1px 0 rgba(196,80,78,0.980392) , 2px 2px 0 rgba(196,80,78,0.960784) , 3px 3px 0 rgba(196,80,78,0.941176) , 4px 4px 0 rgba(196,80,78,0.921569) , 5px 5px 0 rgba(196,80,78,0.901961) , 6px 6px 0 rgba(196,80,78,0.882353) , 7px 7px 0 rgba(196,80,78,0.862745) , 8px 8px 0 rgba(196,80,78,0.843137) , 9px 9px 0 rgba(196,80,78,0.819608) , 10px 10px 0 rgba(196,80,78,0.8) , 11px 11px 0 rgba(196,80,78,0.780392) , 12px 12px 0 rgba(196,80,78,0.760784) , 13px 13px 0 rgba(196,80,78,0.741176) , 14px 14px 0 rgba(196,80,78,0.721569) , 15px 15px 0 rgba(196,80,78,0.701961) , 16px 16px 0 rgba(196,80,78,0.682353) , 17px 17px 0 rgba(196,80,78,0.658824) , 18px 18px 0 rgba(196,80,78,0.639216) , 19px 19px 0 rgba(196,80,78,0.619608) , 20px 20px 0 rgba(196,80,78,0.6) , 21px 21px 0 rgba(196,80,78,0.580392) , 22px 22px 0 rgba(196,80,78,0.560784) , 23px 23px 0 rgba(196,80,78,0.541176) , 24px 24px 0 rgba(196,80,78,0.521569) , 25px 25px 0 rgba(196,80,78,0.498039) , 26px 26px 0 rgba(196,80,78,0.478431) , 27px 27px 0 rgba(196,80,78,0.458824) , 28px 28px 0 rgba(196,80,78,0.439216) , 29px 29px 0 rgba(196,80,78,0.419608) , 30px 30px 0 rgba(196,80,78,0.4) , 31px 31px 0 rgba(196,80,78,0.380392) , 32px 32px 0 rgba(196,80,78,0.360784) , 33px 33px 0 rgba(196,80,78,0.341176) , 34px 34px 0 rgba(196,80,78,0.317647) , 35px 35px 0 rgba(196,80,78,0.298039) , 36px 36px 0 rgba(196,80,78,0.278431) , 37px 37px 0 rgba(196,80,78,0.258824) , 38px 38px 0 rgba(196,80,78,0.239216) , 39px 39px 0 rgba(196,80,78,0.219608) , 40px 40px 0 rgba(196,80,78,0.2) , 41px 41px 0 rgba(196,80,78,0.180392) , 42px 42px 0 rgba(196,80,78,0.156863) , 43px 43px 0 rgba(196,80,78,0.137255) , 44px 44px 0 rgba(196,80,78,0.117647) , 45px 45px 0 rgba(196,80,78,0.0980392) , 46px 46px 0 rgba(196,80,78,0.0784314) , 47px 47px 0 rgba(196,80,78,0.0588235) , 48px 48px 0 rgba(196,80,78,0.0392157) , 50px 50px 0 rgba(196,80,78,0) ;
}

.redbtn:hover {
  background: -webkit-linear-gradient(-90deg, #ce6161 0, #ef6664 100%);
  background: -moz-linear-gradient(180deg, #ce6161 0, #ef6664 100%);
  background: linear-gradient(180deg, #ce6161 0, #ef6664 100%);
  transform: scale(1.1);
}

.redbtn:active {
  background: -webkit-linear-gradient(-90deg, #ff8583 0, #ff5350 100%);
  background: -moz-linear-gradient(180deg, #ff8583 0, #ff5350 100%);
  background: linear-gradient(180deg, #ff8583 0, #ff5350 100%);
  -webkit-box-shadow: 0 5px 0 0 rgba(178,49,49,1) ;
  box-shadow: 0 5px 0 0 rgba(178,49,49,1) ;
}

.feedback-button {
  display: inline-block;
  -webkit-box-sizing: content-box;
  -moz-box-sizing: content-box;
  width: 100px;
  padding: 2% 0;
  margin: 2%;
  overflow: hidden;
  border: none;
  color: #ecf0f1;
  -o-text-overflow: clip;
  text-overflow: clip; 
  background-position: 50% 50%;
  -webkit-background-origin: padding-box;
  background-origin: padding-box;
  -webkit-background-clip: border-box;
  background-clip: border-box;
  -webkit-background-size: auto auto;
  background-size: auto auto; 
}

.feedback-button:hover {
  text-align: center;
  background-position: 50% 50%;
  -webkit-background-origin: padding-box;
  background-origin: padding-box;
  -webkit-background-clip: border-box;
  background-clip: border-box;
  -webkit-background-size: auto auto;
  background-size: auto auto;
}

.feedback-button:active {
  top: 5px;
  background-position: 50% 50%;
  -webkit-background-origin: padding-box;
  background-origin: padding-box;
  -webkit-background-clip: border-box;
  background-clip: border-box;
  -webkit-background-size: auto auto;
  background-size: auto auto;
}

.btnsAll {
  display: flex;
}
              
</style>