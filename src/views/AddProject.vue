<template>
    <div>
        <form @submit.prevent="handleSubmit">
            <lable>Title:</lable>
            <input type="text" v-model="title" required>
            <label>Details:</label>
            <textarea v-model="details" required></textarea>
            <button>Add Project</button>
        </form>
    </div>
</template>

<script setup>
import { ref } from "vue"
import router from '@/router'

    const title = ref('')
    const details = ref('')
    const uri = ref("http://localhost:3000/projects/")

    const handleSubmit = () => {
        let project = {
            title: title.value,
            details: details.value,
            complete: false
        }
        fetch(uri.value , {
            method: 'POST',
            headers: { 'Content-Type': 'application/json'},
            body: JSON.stringify(project)
        })
        .then(() => {
            router.push('/')
            console.log('project is created')
        })
        .catch(error => console.log(error))
    }
</script>

<style scoped>
    form {
        background: white;
        padding: 20px;
        border-radius: 10px;
    }
    label {
        display: block;
        color: #bbb;
        text-transform: uppercase;
        font-size: 14px;
        font-weight: bold;
        letter-spacing: 1px;
        margin: 20px 0 10px 0;
    }
    input {
        padding: 10px;
        border: 0;
        border-bottom: 1px solid #ddd;
        width: 100%;
        box-sizing: border-box;
    }
    textarea {
        border: 1px solid #ddd;
        padding: 10px;
        width: 100%;
        box-sizing: border-box;
        height: 100%;
    }
    form button {
        display: block;
        margin: 20px auto 0;
        background: #00ce89;
        color: white;
        padding: 10px;
        border: 0;
        border-radius: 6px;
        font-size: 16px;
        cursor: pointer;
    }

</style>