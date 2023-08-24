<template>
  <HelloWorld />
</template>

<script setup>
  import HelloWorld from '@/components/HelloWorld.vue'
</script>

<script>
  export default {
    data: () => ({
      breadcrumbs: [
        {
          title: 'Home',
          disabled: true,
          href: '/',
        }
      ],
    }),
  }
</script>