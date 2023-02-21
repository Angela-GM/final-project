<template>
  <div
    class="container-card-task"
    :id="
      props.task.is_complete ? 'bakcground-color-green' : 'bakcground-color-red'
    "
  >
    <h3 :class="props.task.is_complete ? 'task-complete' : 'task-no-complete'">
      {{ task.title }}
    </h3>
    <p :class="props.task.is_complete ? 'task-complete' : 'task-no-complete'">
      {{ task.description }}
    </p>

    <!-- Temporizador de tareas -->
    <div class="temporizador" v-show="!props.task.is_complete">
      <!-- <div>{{ task.name }}</div> -->
      <p id="timer">{{ formatTime(time) }}</p>

      <button v-if="!started" @click="startTimer">
        <i class="fa-solid fa-play"></i>
      </button>
      <button v-if="started" @click="stopTimer">
        <i class="fa-solid fa-stop"></i>
      </button>
    </div>

    <div>
      <!-- Boton de eliminar tarea -->
      <button @click="changeShowModal">
        <img src="https://img.icons8.com/color/48/null/delete-sign--v3.png" />
      </button>

      <!-- Boton de completar tarea cuando se está editando una tarea-->
      <button v-if="inputContainer">
        <img
          id="disable-img2"
          class="disable-img"
          :src="
            props.task.is_complete
              ? 'https://img.icons8.com/color/48/null/checked--v1.png'
              : 'https://img.icons8.com/color/48/null/checked--v3.png'
          "
        />
      </button>

      <!-- Boton de completar tarea normal-->
      <button @click="completeTask" v-else>
        <img
          :src="
            props.task.is_complete
              ? 'https://img.icons8.com/color/48/null/checked--v1.png'
              : 'https://img.icons8.com/color/48/null/checked--v3.png'
          "
        />
      </button>
      <!-- Boton de editar tarea cuando la tarea no está completada-->
      <button @click="showInput" v-if="!props.task.is_complete">
        <img src="https://img.icons8.com/color/48/null/edit--v3.png" />
      </button>
      <!-- Boton de editar tarea cuando la tarea SI está completada-->
      <button v-else>
        <img
          id="disable-img"
          src="https://img.icons8.com/color/48/null/edit--v3.png"
        />
      </button>
    </div>

    <div v-if="inputContainer" class="container-edit">
      <input
        type="text"
        v-model="currentTaskTitle"
        placeholder="Insert title..."
      />
      <textarea
        class="edit-description-task"
        v-model="currentTaskDescription"
        cols="30"
        rows="10"
        type="text"
        placeholder="Add a Task Description"
        maxlength="500"
      ></textarea>

      <!-- <input
        type="text"
        v-model="currentTaskDescription"
        placeholder="Insert description..."
      /> -->
      <button class="button" id="btn-update-task" @click="editTask">
        Update task
      </button>
    </div>
  </div>
  <!-- Ventana Modal al eliminar una tarea -->
  <!-- Overlay -->
  <transition name="fade">
    <div class="modal-overlay" v-if="showModal"></div>
  </transition>

  <transition name="fade">
    <div class="modal-container" v-if="showModal">
      <h1>Are you sure you want to delete:</h1>
      <p>{{ task.title }}</p>
      <button @click="deleteTask">Eliminar</button>
      <button @click="changeShowModal">Cerrar</button>
    </div>
  </transition>
</template>

<script setup>
import { ref } from "vue";
import { useTaskStore } from "../stores/task";
import { supabase } from "../supabase";
//Definir emits para pasar lógica y eventis hacia componentes padres

const emit = defineEmits(["childComplete", "editChild"]);

//funcion para completar tarea que se encarga de enviar la info al padre
const completeTask = () => {
  // console.log(props.task.is_complete);
  emit("childComplete", props.task);
};

//varible para usar tienda de tarea facil
const taskStore = useTaskStore();

// variable para recibir info de la tarea mediante prop como Objeto
const props = defineProps({
  task: Object,
});

// función para mostrar y ocultar imputs
const inputContainer = ref(false);
const currentTaskTitle = ref("");
const currentTaskDescription = ref("");
const showInput = () => {
  console.log("click");
  inputContainer.value = !inputContainer.value;
  currentTaskTitle.value = props.task.title;
  currentTaskDescription.value = props.task.description;
};

// variables para el temporizador

const time = ref(0);
const timer = ref(null);
const started = ref(false);

// Funcion para empezar la cuenta
const startTimer = () => {
  started.value = true;
  timer.value = setInterval(() => {
    time.value += 1;
  }, 1000);
};

// Funcion para parar la cuenta
const stopTimer = () => {
  clearInterval(timer.value);
  started.value = false;
};

// Transformar tiempo en hora, minutos y segundos
const formatTime = (time) => {
  const hours = Math.floor(time / 3600);
  const minutes = Math.floor((time % 3600) / 60);
  const seconds = time % 60;
  return `${hours.toString().padStart(2, "0")}:${minutes
    .toString()
    .padStart(2, "0")}:${seconds.toString().padStart(2, "0")}`;
};

//funcion con validadion+ envio de datos y evento mediante emit

const editTask = () => {
  if (
    currentTaskTitle.value.length === 0 ||
    currentTaskDescription.value.length === 0
  ) {
    alert("Title or Description can not be empty");
  } else {
    const newTaskEdited = {
      title: currentTaskTitle.value,
      description: currentTaskDescription.value,
      id: props.task.id,
    };
    emit("editChild", newTaskEdited);
    inputContainer.value = false;
  }
};

// Función para borrar la tarea a través de la store. El problema que tendremos aquí (y en NewTask.vue) es que cuando modifiquemos la base de datos los cambios no se verán reflejados en el v-for de Home.vue porque no estamos modificando la variable tasks guardada en Home. Usad el emit para cambiar esto y evitar ningún page refresh.

const deleteTask = async () => {
  await taskStore.deleteTask(props.task.id);
};

// Ventana Modal
// variable booleana para mostrar o ocultar el modal
let showModal = ref(false);
const changeShowModal = () => {
  showModal.value = !showModal.value;
};
</script>

<style></style>

<!--
**Hints**
1. ref() or reactive() can be used here to store the following, think if you want to store them either individually or
like an object, up to you.

2. Data properties need here are the following: a boolean to store a false**Important variable, a string to store an error,
a string to store the value of the task that the user can edit, an initial false boolean to hide the inputFIeld used to edit
the new task detail or details[this is in reference of the task title and the task description].

3. Store the custom emit events that will be used to call the functions of the homeView for editing, deleting and toggling the
status[completed, not complted] of the taskItem.

4. Function to handle the error with the data properties used to control the error and the the boolean controlling the boolean
empty variable.

5. Function to handle the edit dialogue where the inputField is displayed and the string used to store the value of the
inputField will be used here to save the value as a prop on this function.

6. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
send via the prop to the parent component. This function can control the toggle completion of the task on the homeview.

7. Function to edit the task information that you decided that the user can edit. This function's body takes in a conditional
that first checks if the value of the task [either title and description or just title] is empty which if true it runs the
function used to handle the error on hint4. Else, the conditional sets the first mentioned boolean data property on hint2
back to its inital boolean value to hide the error message and repeat the same for the data property mentioned 4th on hint2;
a constant that stores an object that holds the oldValue from the prop item and the value of the task coming from the data
property mentioned 3rd on hint2; a custom event emit() that takes 2 parameters a name for the custom event and the value
from the object used on this part of the conditional and lastly this part of the conditional sets the value of input field
to an empty string to clear it from the ui.

8. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
send via the prop to the parent component. This function can control the removal of  the task on the homeview.
-->
