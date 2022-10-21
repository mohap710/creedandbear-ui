<template>
  <div class="mt-2 mb-2">
    <div class="px-0 pt-0 pb-2">
      <ul class="pagination">
        <li class="page-item" v-for="(page,i) in pages" :key="page">
          <a class="page-link" href="javascript:;" @click="goTo(i + 1)">
            {{ i + 1 }}
          </a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  pages: {
    type: Number,
  },
});

const emit = defineEmits(['navigateToPage'])
const goTo = async (page)=>{
    emit("navigateToPage",page)
}
</script>

<style lang="scss" scoped></style>
