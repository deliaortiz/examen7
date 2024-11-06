<template>
    <div class="task-container">
        <h1>Lista de Tareas</h1>

        <!-- Sección para añadir nuevas tareas -->
        <div class="input-group">
            <input 
                v-model="newTask" 
                @keyup.enter="addTask" 
                placeholder="Añadir nueva tarea" 
                class="task-input"
            />
            <button @click="addTask" class="add-button">Añadir</button>
        </div>


        <!-- Mensaje de carga -->
        <div v-if="loading" class="loading">Cargando tareas...</div>
        
        <!-- Mensaje de error -->
        <div v-else-if="error" class="error">{{ error }}</div>
        
        <!-- Lista de tareas -->
        <div v-else-if="tasks.length > 0" class="task-list">
            <div v-for="task in tasks" :key="task.id" class="task-item">
                <div class="task-content">
                    <h5 :style="{ textDecoration: task.completed ? 'line-through' : 'none' }">
                        {{ task.todo }}
                    </h5>
                    <span :class="['status', task.completed ? 'completed' : 'pending']">
                        {{ task.completed ? 'Completada' : 'Pendiente' }}
                    </span>
                    <div class="button-group">
                        <button 
                            @click="toggleTaskCompletion(task)"
                            :class="['toggle-button', task.completed ? 'completed' : 'pending']"
                        >
                            {{ task.completed ? 'Desmarcar' : 'Completar' }}
                        </button>
                        <button @click="deleteTask(task)" class="delete-button">
                            Eliminar
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Mensaje si no hay tareas -->
        <div v-else class="no-tasks">
            No hay tareas disponibles
        </div>
    </div>
</template>

<script>
export default {
    name: "TaskManager",
    data() {
        return {
            newTask: "", // Campo de entrada para la nueva tarea
            tasks: [],   // Lista de tareas locales
            loading: false,
            error: null
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

        async fetchTasks() {
            this.loading = true;
            this.error = null;
            try {
                const response = await fetch('https://dummyjson.com/todos');
                if (!response.ok) {
                    throw new Error('Error al cargar las tareas');
                }
                const data = await response.json();
                this.tasks = data.todos;
            } catch (error) {
                this.error = 'Error al cargar las tareas. Por favor, intente nuevamente.';
                console.error('Error:', error);
            } finally {
                this.loading = false;
            }
        },

        toggleTaskCompletion(task) {
            task.completed = !task.completed;
        },

        deleteTask(task) {
            this.tasks = this.tasks.filter((t) => t.id !== task.id);
        },
    },
    mounted() {
        this.fetchTasks(); // Llama a fetchTasks automáticamente al montar el componente
    }
};
</script>

<style scoped>
.task-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
}

h1 {
    color: #2c3e50;
    text-align: center;
    margin-bottom: 30px;
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
    border-radius: 4px; /* Mantener bordes redondeados */
}

.add-button, .load-button, .toggle-button, .delete-button {
    display: inline-block;
    padding: 12px 30px; /* Ajusta el tamaño del botón */
    font-size: 1rem;
    font-weight: bold;
    color: #fff;
    background: linear-gradient(45deg, #860e88, #9c0cdf); 
    border: none;
    border-radius: 25px; /* Bordes redondeados */
    text-decoration: none;
    transition: background 0.3s ease, transform 0.2s;
}

.add-button:hover, .load-button:hover, .toggle-button:hover, .delete-button:hover {
    background: linear-gradient(45deg, #c7678e, #db8bb7); 
}

.task-list {
    display: flex;
    flex-direction: column;
    gap: 15px; /* Espaciado entre elementos */
}

.task-item {
    background-color: #f9f9f9;
    border: 1px solid #ddd;
    border-radius: 8px; /* Bordes redondeados */
    padding: 15px; /* Espaciado interno */
}

.task-content {
    display: flex;
    flex-direction: column; /* Colocar elementos en columna */
}

.status {
    font-size: 0.9em; /* Tamaño de fuente */
}

.status.completed {
    background-color: #e8f5e9; /* Verde claro */
}

.status.pending {
    background-color: #fff3e0; /* Naranja claro */
}

.button-group {
    display: flex; /* Flexbox para botones */
}

.loading, .error, .no-tasks {
   text-align:center; /* Centrar texto */
}
.loading { 
   color:#666; 
}
.error { 
   color:#f44336; 
   background-color:#ffebee; 
   border-radius :4 px ; 
}
.no-tasks { 
   color:#666; 
   background-color:#f5f5f5; 
   border-radius :4 px ; 
}
</style>