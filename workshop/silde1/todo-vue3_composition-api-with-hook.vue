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
@import 'style.css';
</style>
