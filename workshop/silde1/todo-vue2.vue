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
export default {
    props: {
        title: String,
    },
    data() {
        return {
            todo: '',
            items: ['Vue', 'is', 'Awesome'],
        };
    },
    methods: {
        // Add: Click Handler Function
        add() {
            if (this.todo) {
                this.items.push(this.todo);
                this.todo = '';
            }
        },

        // Remove: Click Handler Function
        remove(item) {
            this.items = this.items.filter((v) => v !== item);
        },
    },
    // mounted hook
    mounted() {
        console.log(`mounted ! ${this.title}`);
    },
};
</script>

<style>
@import 'style.css';
</style>
