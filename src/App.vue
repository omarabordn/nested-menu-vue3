<template>
  <div class="flex">
    <div class="w-1/4 p-4 border-r">
      <h2 class="font-bold mb-4">Pages</h2>
      <ul>
        <li
          v-for="page in pages"
          :key="page.id"
          @click="addPageToMenu(page)"
          class="cursor-pointer p-2 bg-gray-100 mb-2 rounded hover:bg-gray-200" >
          {{ page.name }}
        </li>

      </ul>
    </div>



    <div class="w-3/4 p-4">
      <h2 class="font-bold mb-4">Menu Builder</h2>
      <ul id="menu-list" class="p-4 border border-gray-300 rounded bg-white">
         <MenuItem
          v-for="(item, index) in menu"
          :key="item.id"
          :item="item"
          :index="index"
          @remove-item="removeMenuItem"/>
      </ul>

    </div>
  </div>

</template>

<script lang="ts">
import { defineComponent, reactive, onMounted } from 'vue';
import MenuItem from './components/MenuItem.vue';
import Sortable from 'sortablejs';

interface Page {
  id: number;
  name: string;
}

interface MenuItem {
  id: number;
  name: string;
  children: MenuItem[];
}

export default defineComponent({
  name: 'App',
  components: {
    MenuItem,
  },
  setup() {
    const pages = reactive<Page[]>([
      { id: 1, name: 'Home' },
      { id: 2, name: 'About Us' },
      { id: 3, name: 'Services' },
      { id: 4, name: 'Contact' },
    ]);

    const menu = reactive<MenuItem[]>([]);

    const addPageToMenu = (page: Page) => {
      menu.push({ id: Date.now(), name: page.name, children: [] });
    };

    const removeMenuItem = (index: number) => {
      menu.splice(index, 1);
    };

    onMounted(() => {
      Sortable.create(document.getElementById('menu-list')!, {
        group: 'nested',
        animation: 150,
        onEnd(evt) {
          const [removed] = menu.splice(evt.oldIndex!, 1);
          menu.splice(evt.newIndex!, 0, removed);
        },
      });
    });

    return {
      pages,
      menu,
      addPageToMenu,
      removeMenuItem,
    };
  },
});

</script>

<style scoped>

</style>
