<template>
    <div class="row">
        <div class="col-md-6 offset-md-3 border d-flex justify-content-between text-center pt-2 pb-2">
        <span>{{ todo.suser }} - {{ todo.text }}</span>
        <p style="display:none;">{{ todo.id }}</p>
        <button @click="$emit('removeItem', todo.id)" class="btn btn-danger">Delete</button>
        </div>
    </div> 
</template>

<script>
export default {
    props: ["todo"]
}
</script>