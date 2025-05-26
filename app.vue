<template>
    <div>
        <NuxtLayout>
            <NuxtPage />
        </NuxtLayout>
    </div>
</template>
<script setup>
onMounted(() => {
    document.documentElement.setAttribute('data-theme', 'dim');
});
</script>