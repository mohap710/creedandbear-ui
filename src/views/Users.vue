<template>
  <Suspense>
    <template #default>
      <users-table />
    </template>
    <template #fallback>
      <UsersTableSkeleton />
    </template>
  </Suspense>
</template>

<script setup>
import UsersTable from "@/views/components/UsersTable.vue";
import UsersTableSkeleton from "@/components/skeletons/UsersTable.vue";

</script>

