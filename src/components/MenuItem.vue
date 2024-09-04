<template>
  <li class="mb-2">
    <div class="flex justify-between items-center p-2 bg-gray-100 rounded">
      {{ item.name }}
      <button @click="removeItem" class="text-red-500 ml-2">Remove</button>
    </div>

    <ul class="ml-4 mt-2" v-if="item.children.length">
      <MenuItem
        v-for="(child, index) in item.children"
        :key="child.id"
        :item="child"
        :index="index"
        @remove-item="removeChild"/>
    </ul>
  </li>
  
</template>

<script lang="ts">
import { defineComponent } from 'vue';

interface MenuItem {
  id: number;
  name: string;
  children: MenuItem[];
}

export default defineComponent({
  name: 'MenuItem',
  props: {
    item: {
      type: Object as () => MenuItem,
      required: true,
    },

    index: {
      type: Number,
      required: true,
    },
  },

  emits: ['remove-item'],
  methods: {
    removeItem() {
      this.$emit('remove-item', this.index);
    },
    removeChild(childIndex: number) {
      this.item.children.splice(childIndex, 1);
    },
  },
  
});

</script>
<style scoped>
</style>
