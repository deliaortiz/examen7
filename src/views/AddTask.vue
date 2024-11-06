<template>
    <div class="add-task-container">
        <h1>Añadir Tarea</h1>
        <div class="input-group">
            <input 
                v-model="newTask" 
                @keyup.enter="addTask" 
                placeholder="Añadir nueva tarea" 
                class="task-input"
            />
            <button @click="addTask" class="add-button">Añadir</button>
        </div>

        <div v-if="tasks.length > 0" class="task-list">
            <div v-for="task in tasks" :key="task.id" class="task-item">
                <span :class="{ completed: task.completed }">{{ task.todo }}</span>
                <div>
             <button @click="toggleTaskCompletion(task)" class="toggle-button">
                 {{ task.completed ? 'Desmarcar' : 'Completar' }}
                 </button>
                <button @click="deleteTask(task)" class="delete-button">Eliminar</button>
            </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "AddTask",
    data() {
        return {
            newTask: "", // Campo de entrada para la nueva tarea
            tasks: [],   // Lista de tareas locales
        };
    },
    methods: {
        addTask() {
            if (this.newTask.trim() === "") return;

            const newTask = {
                todo: this.newTask,
                completed: false,
                id: Date.now(), 
            };

            // Añadir la nueva tarea al inicio de la lista
            this.tasks.unshift(newTask);
            this.newTask = ""; // Limpiar el campo de entrada después de agregar
        },

        // Elimina una tarea específica de la lista
        deleteTask(task) {
            this.tasks = this.tasks.filter((t) => t.id !== task.id);
        },

        // Cambia el estado de la tarea entre completada y no completada
        toggleTaskCompletion(task) {
            task.completed = !task.completed;
        },
    },
};
</script>

<style scoped>
.add-task-container {
    padding: 20px;
    max-width: 400px;
    margin: 0 auto;
}

.input-group {
    display: flex;
    margin-bottom: 10px;
}

.task-input {
    flex-grow: 1;
    padding: 8px;
    margin-right: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

.add-button {
    display: inline-block;
    padding: 12px 30px;
    font-size: 1rem;
    font-weight: bold;
    color: #fff;
    background-color: #4CAF50; /* Verde para añadir */
    border: none;
    border-radius: 25px; /* Bordes redondeados */
    transition: background 0.3s ease, transform 0.2s;
}

.add-button:hover {
    background-color: #45a049; /* Verde más oscuro al pasar el mouse */
}

.task-list {
    margin-top: 20px;
}

.task-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    border-bottom: 1px solid #eee;
}

.completed {
    text-decoration: line-through;
    color: rgb(84, 35, 35);
}

/* Botón para completar/desmarcar tareas */
.toggle-button {
    padding: 8px 16px; 
    border-radius: 25px; /* Bordes redondeados */
    color: white; /* Texto blanco */
}

/* Color amarillo para tareas pendientes */
.toggle-button:not(.completed) {
   background-color:#FFC107; /* Amarillo para pendientes */
}

/* Color naranja para tareas completadas */
.toggle-button.completed {
   background-color:#FF9800; /* Naranja para completadas */
}

/* Estilos para el botón eliminar */
.delete-button {
   padding: 8px 16px; 
   background-color:#f44336; /* Rojo para eliminar */
   color:white; /* Texto blanco */
   border-radius: 25%; /* Sin borde */
   border-radius:25px; /* Bordes redondeados */
}

.delete-button:hover {
   background-color:#d32f2f; /* Rojo más oscuro al pasar el mouse */
}
</style>