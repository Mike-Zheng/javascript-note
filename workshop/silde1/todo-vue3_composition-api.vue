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
import { toRefs, ref, onMounted } from 'vue';
export default {
    props: ['title'],
    setup(props, context) {
        const { title } = toRefs(props);
        const todo = ref('');
        const items = ref(['Vue', 'is', 'Awesome']);
        // Add: Click Handler Function
        const add = () => {
            if (todo.value) {
                items.value.push(todo.value);
                todo.value = '';
            }
        };
        // Remove: Click Handler Function
        const remove = (item) => {
            items.value = items.value.filter((v) => v !== item);
        };
        // mounted hook
        onMounted(() => {
            console.log(`onMounted ! ${title.value} `);
        });

        return {
            todo,
            items,
            add,
            remove,
            title,
        };
    },
};
</script>
<style>
@import 'style.css';
</style>
