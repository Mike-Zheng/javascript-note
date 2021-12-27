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

<script setup>
import { toRefs } from 'vue';
import hook from './hook.js';
const props = defineProps({ title: String });
const { todo, items, add, remove } = toRefs(hook(props.title));
</script>

<style>
@import 'style.css';
</style>
