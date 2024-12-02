<script>
    import Todo from './Todo.svelte';
    import '../app.css';
    import underline from "$lib/img/underline.png"
</script>

    <h2>Self Care</h2>
    <img class="underline" src="{underline}" alt="trash"/>
    <h1>TO DO LIST</h1>

<Todo />

<style>
    .underline {
        width: 35%;
        opacity: 30%;
    }
</style>