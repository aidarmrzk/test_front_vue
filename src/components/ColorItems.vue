<template>
    <div class="container" :class="{ 'open': openList }">
        <button @click="handleSort">
            <span v-if="isMix">Сортировать</span>
            <span v-else>Перемешать</span>
        </button>
        <span>{{ model.list_name }}</span>
        <div class="color-container">
            <div class="color-mix-container" v-if="isMix">
                <template v-for="(item, index) in itemList" :key="index">
                        <div class="inner-block" :style="{ backgroundColor: item.color }" @click="deleteElement(item)"></div>
                </template>
            </div>
            <template v-else>
                <div v-for="(item, index) in model.items" :key="index">
                    <div v-if="item.status" class="block-color">
                        <div v-for="n in item.count" :key="n" class="inner-block" :style="{ backgroundColor: item.color }" @click="deleteElement(item)"></div>
                    </div>
                </div>
            </template>
        </div>
    </div>
</template>

<script>
import { ref, onMounted, watch } from 'vue';

export default {
    emits: ['deleteElement'],
    props: {
        model: {
            required: true,
        }
    },
    setup(props, { emit }) {
        const openList = ref(false);
        const isMix = ref(false);
        const itemList = ref([]);

        const deleteElement = (item) => {
            emit('deleteElement', item);
        }

        const handleSort = () => {
            isMix.value = !isMix.value
        }

        const shuffleArray = (array) => {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        };

        const addItemList = () => {
            itemList.value = [];
            props.model.items.forEach((item) => {
                if (item.status) { 
                    for (let i = 0; i < item.count; i++) {
                        itemList.value.push({
                        id: item.id,
                        color: item.color,
                        status: item.status,
                        });
                    }
                }
            });
            shuffleArray(itemList.value)
        };

        onMounted(() => {
            addItemList(); 
        });

        watch(
            () => props.model.items.map(item => item),
            () => {
                addItemList();
            },
            { deep: true }
        );

        return {
            openList,
            isMix,
            itemList,
            deleteElement,
            handleSort
        };
    }
}
</script>

<style scoped>
.container {
    position: relative;
    border: 1px solid #000;
    padding: 10px;
}
.color-container {
    display: flex;
    flex-direction: column;
    margin: 10px 0 0;
}
.block-color,
.color-mix-container {
    display: flex;
    flex-wrap: wrap;
    margin: 10px 0 0;
}
.inner-block {
    width: 20px;
    height: 20px;
    margin: 2px;
    cursor: pointer;
}
button {
    position: absolute;
    top: 10px;
    right: 10px;
    background-color: #0b9df1;
    border: none;
    color: #fff;
    padding: 5px 8px;
    border-radius: 5px;
    cursor: pointer;
}
</style>  