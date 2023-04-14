<!-- eslint-disable vue/require-v-for-key -->
<template>
    <div>
        <h1>Ma Todo List</h1>

        <!-- Formulaire d'ajout de tâches -->
        <form @submit.prevent="addTask">
            <label for="new-task" style="text-align: center; line-height: 50px;">Ajouter une tâche :</label>
            <input type="text" id="new-task" v-model="newTask">
            <select v-model="taskResponsible" style="margin-right: 8px;">
                <option value="">Assigné à</option>
                <option v-for="name in fakeNames" :value="name">{{ name }}</option>
            </select>
            <button type="submit"
                style="background-color: #green; color: #fff; border: none; padding: 8px; cursor: pointer;">Ajouter</button>

        </form>

        <!-- Liste de tâches -->
        <ul>
            <li v-for="(task, index) in filteredTasks" :key="index" :class="{ completed: task.completed }">

                <!-- Checkbox pour sélectionner/désélectionner la tâche -->
                <input type="checkbox" v-model="selectedTasks" :value="index">

                <!-- Champ de texte pour éditer la tâche -->
                <input type="text" v-model="task.text" @blur="updateTask(task)">

                <!-- Liste déroulante pour assigner une personne à la tâche -->
                <select v-model="task.assignedTo">
                    <option value="">Assigné à</option>
                    <option v-for="name in fakeNames" :value="name">{{ name }}</option>

                </select>

                <!-- Champs texte pour ajouter du temps -->
                <input type="number" v-model="task.timeImp" @blur="updateTask(task)" min="0" />

                <!-- bouton pour valider la tache -->
                <button @click="toggleCompleted(task)" :class="{ completed: task.completed }">
                    {{ task.completed ? 'Non terminée' : 'Terminée' }}
                </button>

                <!-- Bouton pour supprimer la tâche -->
                <button @click="deleteTask(index)">Supprimer</button>


            </li>
        </ul>

        <!-- Bouton pour supprimer les tâches sélectionnées -->
        <button @click="deleteSelectedTasks"
            style="background-color: #dc3545; color: #fff; border: none; padding: 8px; cursor: pointer;">Supprimer
            les tâches sélectionnées</button>

        <!-- Filtres -->
        <div>
            <label for="filter">Filtre :</label>
            <select id="filter" v-model="currentFilter">
                <option value="all">Toutes</option>
                <option value="completed">Terminées</option>
                <option value="uncompleted">Non terminées</option>
            </select>
        </div>

        <!-- Compteur de tâches -->
        <div>
            <p>Nombre de tâches : {{ tasks.length }}</p>
            <p>Nombre de tâches sélectionnées : {{ selectedTasks.length }}</p>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            tasks: [
            ],
            newTask: '',
            selectedTasks: [],
            currentFilter: 'all',
            fakeNames: ['Alex', 'Jean', 'Mark', 'Kylian'],
            taskResponsible: ''
        }
    },
    computed: {
        // Filtrer les tâches en fonction du filtre sélectionné
        filteredTasks() {
            if (this.currentFilter === 'completed') {
                return this.tasks.filter(task => task.completed)
            } else if (this.currentFilter === 'uncompleted') {
                return this.tasks.filter(task => !task.completed)
            } else {
                return this.tasks
            }
        }
    },
    methods: {
        // Ajouter une tâche
        addTask() {
            if (this.newTask.trim() && this.taskResponsible) {
                this.tasks.push({ text: this.newTask, completed: false, assignedTo: this.taskResponsible });
                this.newTask = '';
                this.taskResponsible = '';
            } else {
                // Afficher un message d'erreur si les champs sont vides
                alert('Veuillez renseigner le nom de la tâche et son auteur s il vous plait.');
            }
        },
        // Supprimer une tâche
        deleteTask(index) {
            this.tasks.splice(index, 1)
        },
        // Supprimer les tâches sélectionnées
        deleteSelectedTasks() {
            this.selectedTasks.sort().reverse().forEach(index => {
                this.tasks.splice(index, 1)
            })
            this.selectedTasks = []
        },
        onTaskSelectionChange(task) {
            if (task.selected) {
                this.selectedTasks.push(task);
            } else {
                this.selectedTasks = this.selectedTasks.filter((t) => t !== task);
            }
        },
        // Mettre à jour une tâche
        updateTask(task) {
            // On ne met pas à jour la tâche si elle est vide
            if (!task.text.trim()) {
                return
            }
            task.completed = false;
        },
        toggleCompleted(task) {
            task.completed = !task.completed;
        },
    }
}
</script>

<style>
h1 {
    font-size: 2rem;
    margin-bottom: 1rem;
}

form {
    display: flex;
    margin-bottom: 1rem;
}

label {
    font-size: 1.2rem;
    margin-right: 1rem;
}

input[type="text"] {
    font-size: 1.2rem;
    padding: 0.5rem;
    margin-right: 1rem;
    border: 1px solid #ccc;
    border-radius: 3px;
}

button[type="submit"],
button {
    font-size: 1.2rem;
    padding: 0.5rem 1rem;
    color: #fff;
    border: none;
    border-radius: 3px;
    cursor: pointer;
}

ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
}

li {
    display: flex;
    align-items: center;
    margin-bottom: 0.5rem;
    font-size: 1.2rem;
}

input[type="checkbox"] {
    margin-right: 1rem;
    cursor: pointer;
}

input[type="text"] {
    flex: 1;
}

input[type="number"] {
    font-size: 1.2rem;
    padding: 0.5rem;
    margin-right: 1.2rem;
    border: 1px solid #ccc;
    border-radius: 3px;
    margin-left: 0.5rem;
}

button {
    margin-left: 1rem;
    margin-bottom: 1rem;
    background-color: #e74c3c;
}

select {
    font-size: 1.2rem;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 3px;
}

p {
    font-size: 1.2rem;
    margin-bottom: 0.5rem;
}

.completed {
    background-color: #27ae60;
}

.task-checkbox {
    margin-right: 10px;
    /* Ajoutez d'autres styles si nécessaire */
}</style>