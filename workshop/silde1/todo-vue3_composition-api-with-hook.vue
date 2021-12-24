<template>
    <div class="todo-warpper">
        <div class="header">
            <h2>{{ title }}</h2>

            <div class="create-wrapper">
                <input v-model="todo" @keyup.enter="add" type="text" class="create-wrapper__input" />
                <button class="create-wrapper__button" @click="add">Add</button>
            </div>
        </div>

        <div class="list-group">
            <div class="list-group-item" v-for="item in items" :key="item">
                <div></div>
                <span>{{ item }}</span>
                <button class="close" @click="remove(item)">
                    <span>&times;</span>
                </button>
            </div>
        </div>
    </div>
</template>

<script>
import toDo from './hook.js';

export default {
    props: ['title'],
    setup(props, context) {
        const todo = toDo(props.title);

        return todo;
    },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500&display=swap');
* {
    padding: 0;
    margin: 0;
    font-family: 'Poppins', sans-serif;
    outline: none;
    border: none;
    border-radius: 0;
    color: #000000bd;
}
body {
    position: absolute;
    width: 100%;
    height: 100%;
    font-size: 0.95rem;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #0093e9;
    background-image: linear-gradient(160deg, #0093e9 0%, #80d0c7 100%);
}
.todo-warpper {
    position: relative;
    display: grid;
    background: #ffff;
    box-shadow: 0 10px 80px #0000003d;
    border-radius: 1rem;
    overflow-y: hidden;
}

.header {
    padding: 20px 20px 10px 20px;
    border-bottom: 1px solid #bbbdc7;
}

h2 {
    margin: 0;
    padding: 0;
    text-align: left;
    font-weight: bold;
}

.create-wrapper {
    display: grid;
    gap: 20px;
    grid-template-columns: 1fr auto;
    margin: 20px 0 0 0;
}

.create-wrapper__input {
    background: transparent;
    width: 100%;
    border-radius: 0;
    font-size: 0.95rem;
}

.create-wrapper__button {
    padding: 5px 15px;
    background: #eee;
    border-radius: 1rem;
    cursor: pointer;

    font-size: 0.95rem;
}

.list-group {
    overflow-y: auto;
    display: grid;
    margin: 0 5px 0 0;
}

.list-group-item {
    display: grid;
    grid-template-columns: auto 1fr auto;
    align-items: center;
    gap: 15px;
    padding: 10px 20px;
    border-bottom: 1px solid #bbbdc750;
}
.list-group-item span {
    overflow-x: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    text-align: left;
}

.close {
    border-radius: 20px;
    padding: 1px 5px;
}
</style>
