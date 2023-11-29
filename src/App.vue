<template>
  <div class="wrapper">
    <ul class="color-list">
      <li v-for="color in colorList" :key="color.id">
        <ListItems
        :model="color" 
        @changeStatusItem="changeStatusItem" 
        @changeColorItem="changeColorItem" 
        @changeCountItem="changeCountItem"
        @changeStatusList="changeStatusList"/>
      </li>
    </ul>
    <ul class="color-list flex">
      <li v-for="color in colorList" :key="color.id">
        <ColorItems :model="color" @deleteElement="deleteElement" />
      </li>
    </ul>
  </div>
</template>

<script>
import ListItems from './components/ListItems'
import ColorItems from './components/ColorItems'
import { ref } from 'vue';

export default {
  name: 'App',
  components: {
    ListItems,
    ColorItems
  },

  setup() {
    const generateUniqueId = () => {
        const timestamp = Date.now();
        const randomNum = Math.floor(Math.random() * 10000);
        return `${timestamp}-${randomNum}`;
    }

    const colorList = ref([
      {
        list_name:'List 1',
        id_list: generateUniqueId(),
        items: [
          {item_name:'Item 1', id:generateUniqueId(), count: 10, color: '#de2d2d', status: true},
          {item_name:'Item 2', id:generateUniqueId(), count: 16, color: '#e7d429', status: true},
          {item_name:'Item 3', id:generateUniqueId(), count: 40, color: '#11a820', status: true},
          {item_name:'Item 4', id:generateUniqueId(), count: 23, color: '#1120a8', status: true},
          ],
        sort_status: false
      },
      {
        list_name:'List 2',
        id_list: generateUniqueId(),
        items: [
          {item_name:'Item 1', id:generateUniqueId(), count: 4, color: '#de2d2d', status: false},
          {item_name:'Item 2', id:generateUniqueId(), count: 31, color: '#e7d429', status: true},
          {item_name:'Item 3', id:generateUniqueId(), count: 57, color: '#11a820', status: false},
          {item_name:'Item 4', id:generateUniqueId(), count: 18, color: '#1120a8', status: true},
          ],
        sort_status: false
      },
      {
        list_name:'List 3',
        id_list: generateUniqueId(),
        items: [
          {item_name:'Item 1', id:generateUniqueId(), count: 1, color: '#e7d429', status: true},
          {item_name:'Item 2', id:generateUniqueId(), count: 14, color: '#de2d2d', status: false},
          {item_name:'Item 3', id:generateUniqueId(), count: 32, color: '#11a820', status: true},
          {item_name:'Item 4', id:generateUniqueId(), count: 17, color: '#1120a8', status: false},
          ],
        sort_status: false
      },
      {
        list_name:'List 4',
        id_list: generateUniqueId(),
        items: [
          {item_name:'Item 1', id:generateUniqueId(), count: 35, color: '#11a820', status: false},
          {item_name:'Item 2', id:generateUniqueId(), count: 5, color: '#e7d429', status: false},
          {item_name:'Item 3', id:generateUniqueId(), count: 13, color: '#de2d2d', status: false},
          {item_name:'Item 4', id:generateUniqueId(), count: 23, color: '#1120a8', status: true},
          ],
        sort_status: false
      },
    ])

    const findItem = (selectedItem) => {
      const targetList = colorList.value.find((list) => list.items.find((item) => item.id === selectedItem.id));

      if (targetList) {
        const targetItem = targetList.items.find((item) => item.id === selectedItem.id);
        return targetItem;
      }
    }

    const changeStatusItem = (selectedItem) => {
      const targetItem = findItem(selectedItem)
      if (targetItem) {
        targetItem.status = selectedItem.status;
      }
    };

    const changeCountItem = (selectedItem) => {
      const targetItem = findItem(selectedItem)
      if (targetItem) {
        targetItem.count = selectedItem.count;
      }
    };

    const changeColorItem = (selectedItem) => {
      const targetItem = findItem(selectedItem)
      if (targetItem) {
        targetItem.color = selectedItem.color;
      }
    };

    const changeStatusList = (selectedList) => {
      const targetList = colorList.value.find((list) => list.id_list === selectedList.idList);

      if (targetList) {
        targetList.items.forEach((item) => {
          item.status = selectedList.checkStatus;
        });
      }
    }

    const deleteElement = (selectedItem) => {
      const targetItem = findItem(selectedItem);

      if (targetItem) {
        targetItem.count -= 1;
      }
    }

    return {
      colorList,
      changeStatusItem,
      changeCountItem,
      changeColorItem,
      changeStatusList,
      deleteElement
    }
  }
}
</script>

<style>
* {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.wrapper {
  display: flex;
  gap: 20px;
  padding: 20px;
  width: 100%;
}
.color-list {
  border: 1px solid #000;
  padding: 20px;
  list-style: none;
  width: 100%;
}
.flex {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
</style>
