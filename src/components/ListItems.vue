<template>
    <ul :class="{ 'open': openList }">
        <div class="list-header">
            <img src="@/assets/arrow.svg" alt="" @click="toggleList">
            <input type="checkbox" :checked="statusCheck" :class="{ 'point': someCheck }" @change="changeStatusList(model)">
            <span>{{ model.list_name }}</span>
        </div>
        <li v-for="(item, index) in model.items" :key="index" :class="{ 'last': index === model.items.length - 1 }">
            <div class="color-header">
                <input type="checkbox" :checked="item.status" @change="changeStatusItem(item)">
                <span>{{ item.item_name }}</span>
            </div>
            <div class="color-box">
                <input v-model="item.count" type="number" min="0" @input="changeCountItem(item)">
                <input v-model="item.color" type="color" @input="changeColorItem(item)">
            </div>
        </li>
    </ul>
</template>

<script>
import { ref, onMounted , watch  } from 'vue';

export default {
    emits: ['changeStatusItem', 'changeColorItem', 'changeCountItem', 'changeStatusList'],
    props: {
        model: {
            required: true,
        }
    },
    setup(props, { emit }) {
        const statusCheck = ref();
        const someCheck = ref();
        const openList = ref(false);

        const changeStatusItem = (item) => {
            item.status = !item.status;
            emit('changeStatusItem', item);
        };

        const changeCountItem = (item) => {
            item.count = Math.abs(parseInt(item.count, 10)) || 0;
            emit('changeCountItem', item);
        };

        const changeColorItem = (item) => {
            emit('changeColorItem', item);
        };

        const updateStatusCheck = () => {
            const allItemsSelected = props.model.items.every((item) => item.status);
            const someItemsSelected = props.model.items.some((item) => item.status);

            statusCheck.value = allItemsSelected;

            if (someItemsSelected && !allItemsSelected) {
                someCheck.value = true;
            } else {
                someCheck.value = false;
            }
        };

        const changeStatusList = (model) => {
            statusCheck.value = !statusCheck.value
            const checkStatus = statusCheck.value
            const idList = model.id_list
            emit('changeStatusList', {idList, checkStatus});
        } 

        const toggleList = () => {
            openList.value = !openList.value
        }

        onMounted(() => {
            updateStatusCheck();
        });

        watch(() => props.model.items, updateStatusCheck, { deep: true });

        return {
            changeStatusItem,
            changeColorItem,
            changeCountItem,
            changeStatusList,
            statusCheck,
            someCheck,
            openList,
            toggleList
        };
    }
}
</script>

<style scoped>
ul {
    list-style: none;
}
.open img {
    transform: none;
}
img {
    width: 15px;
    height: 15px;
    transform: rotate(270deg);
    cursor: pointer;
}
span {
    white-space: nowrap;
}
.list-header {
    display: flex;
    align-items: center;
    gap: 5px;
    margin: 0 0 10px;
}
.color-header {
    display: flex;
    align-items: center;
    gap: 5px;  
}
input[type="checkbox"] {
    width: 20px;
    height: 20px;
}
.open li {
    max-height: 100%;
    padding: 5px 0 5px 40px;
    overflow: auto;
}
li {
    max-height: 0;
    overflow: hidden;
    padding: 0 0 0 40px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 80%;
}
li.last {
    margin-bottom: 10px;
}
.color-box {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    gap: 10px;
}
input[type="number"] {
    width: 50px;
    border: none;
    font-size: 16px;
    text-align: end;
}
input[type="color"] {
    width: 25px;
    height: 27px;
    border: none;
    background-color: #fff;
}
.list-header input {
    position: relative;
}
.list-header input.point::before {
    content: '\2022';
    display: block;
    width: 100%;
    height: 100%;
    text-align: center;
    line-height: 21px;
    position: absolute;
    top: 0;
    left: 0;
    color: #000;
}
</style>  