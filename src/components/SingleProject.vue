<template>
    <div class="project" :class="{ complete: project.complete }">
        <div class="actions">
            <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
            <div class="icons">
                <router-link :to="{name : 'EditProject', params: {id: project.id}}">
                    <span class="material-symbols-outlined material-icons">edit</span>
                </router-link>
                <span class="material-symbols-outlined material-icons" @click="deleteProject(project.id)">delete</span>
                <span class="material-symbols-outlined material-icons tick" @click="toggleComplete(project)">done</span>
            </div>
        </div>
        <div class="details" v-if="showDetails">
            <p>{{ project.details }}</p>
        </div>
    </div>
</template>
<script setup>
import { ref } from "vue";
const emits = defineEmits(['customEvent']);
const props = defineProps({
    project: {
        type: Object,
    }
});

    const showDetails = ref(false)
    const uri = ref("http://localhost:3000/projects/")

    const deleteProject = (id) => {
        fetch(uri.value + id,{method: 'DELETE'})
        .then(() => emits('delete',id))
        .catch(error => console.log(error))
    }
    const toggleComplete = (project) => {
        fetch(uri.value + project.id,{
            method: 'PATCH',
            headers: { 'Content-Type': 'application/json'},
            body: JSON.stringify({complete: !project.complete})
        })
        .then(() => emits('complete',project.id))
        .catch(error => console.log(error))
    }
</script>
<style scoped>
    .project {
        margin: 20px auto;
        background: white;
        padding: 10px 20px;
        border-radius: 4px;
        box-shadow: 1px 2px 3px rgb(0,0,0,0.05);
        border-left: 4px solid #e90074
    }
    .project.complete {
        border-left: 4px solid #00ce89
    }
    .project.complete .tick {
       color: #00ce89
    }
    h3 {
        cursor: pointer;
    }
    .actions {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .material-icons {
        font-size: 24px;
        margin-left: 10px;
        color: #bbb;
        cursor: pointer;
    }
    .material-icons:hover {
        color: #777
    }
</style>