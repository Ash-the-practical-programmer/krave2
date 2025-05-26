<template>
    <div :class="[
        'message p-2',
        sender === 'user' ? 'ml-auto' : 'mr-auto'
    ]">
        <div :class="[
            'max-w-[280px] sm:max-w-[320px] p-3 rounded-lg',
            sender === 'user' ? 'bg-primary text-primary-content ml-auto' : 'bg-base-200 text-base-content'
        ]">
            <div v-if="text" class="text-sm leading-relaxed">{{ text }}</div>
            <div v-if="images && images.length" class="mt-2 flex flex-wrap gap-2">
                <img v-for="image in images" :key="image.name" :src="getImageUrl(image)" :alt="`Food image: ${image.name}`"
                    loading="lazy"
                    class="w-20 h-20 object-cover rounded-md" />
            </div>
        </div>
    </div>
</template>

<script setup>
defineProps({
    sender: { type: String, required: true },
    text: { type: String, default: '' },
    images: { type: Array, default: () => [] },
})

const imageUrls = ref({})

const getImageUrl = (file) => {
    if (!imageUrls.value[file.name]) {
        imageUrls.value[file.name] = URL.createObjectURL(file)
    }
    return imageUrls.value[file.name]
}

onUnmounted(() => {
    Object.values(imageUrls.value).forEach((url) => URL.revokeObjectURL(url))
})
</script>