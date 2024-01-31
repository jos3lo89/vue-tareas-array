<script setup>
import { reactive, ref } from 'vue';

const tituloTarea = ref('')
const tareaTexto = ref('')
const mensaje = ref(false)


const listaTareas = reactive([])

const agregarTarea = () => {
    if (tituloTarea.value === '' || tareaTexto.value === '') {
        mensaje.value = true
    } else {
        listaTareas.push({
            id: crypto.randomUUID(),
            titulo: tituloTarea.value,
            tarea: tareaTexto.value,
            cumplida: false
        });

        tareaTexto.value = ''
        tituloTarea.value = ''
        mensaje.value = false

    }

};

const cumplidaTarea = (id) => {
    const tareEncontrada = listaTareas.find(tarea => tarea.id === id)
    if (tareEncontrada) {
        tareEncontrada.cumplida = !tareEncontrada.cumplida
    } else {
        console.log("tarea no encontrada")
    }
}

const eliminarTarea = (id) => {
    const index = listaTareas.findIndex(tarea => tarea.id === id);
    if (index !== -1) {
        listaTareas.splice(index, 1);
    } else {
        console.log("tarea no encontrada");
    }
};
</script>

<template>
    <div class="box">
        <h2>Agregar nuevas tareas</h2>
        <input type="text" v-model="tituloTarea" placeholder="Titulo de la tarea" name="tituloDeTarea">
        <br>
        <br>
        <textarea v-model="tareaTexto" placeholder="Escriba la tarea" name="textDeTarea"></textarea>
        <div>
            <button v-on:click="agregarTarea(tituloTarea, tareaTexto)">Guardar</button>
        </div>
        <span v-if="mensaje" class="mensajeee">Ingresa todos los compos baboso zzz</span>

    </div>
    <div v-for="tar in listaTareas" :key="tar.id" class="box2">
        <h4 :class="tar.cumplida ? 'raya' : ''" class="tituloTarea">{{ tar.titulo }}</h4>
        <p :class="tar.cumplida ? 'raya' : ''">{{ tar.tarea }}</p>
        <div>
            <button v-on:click="cumplidaTarea(tar.id)" :class="tar.cumplida ? 'cumplida' : ''">Cumplida</button>
            <button v-on:click="eliminarTarea(tar.id)" class="borra">borra</button>
        </div>
    </div>

</template>

<style scoped>
.tituloTarea {
    color: #219ebc;
    text-transform: capitalize;
}

.box {
    max-width: 600px;
    margin: 0 auto;
    text-align: center;
    background-color: #303030;
    padding: 20px;
    margin-top: 20px;
    border-radius: 30px;
}

.box2 {
    max-width: 600px;
    margin: 0 auto;
    text-align: center;
    background-color: #303030;
    margin-top: 10px;
    padding: 10px;
    border-radius: 30px;
}

button {
    cursor: pointer;
    border: none;
    margin: 0 5px 0 5px;
    padding: 3px;
}

.raya {
    text-decoration-line: line-through;
}

.cumplida {
    background: yellow;
    color: black;
}

.borra:hover {
    background: red;
}

.mensajeee {
    color: red;
}

input,
textarea {
    padding: 5px;
    border: none;
}
</style>