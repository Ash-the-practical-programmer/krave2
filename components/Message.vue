<template>
    <div :class="[
        'message-container max-w-[85%] transition-all duration-300 animate-slide-up font-sans',
        sender === 'user' ? 'ml-auto' : 'mr-auto'
    ]">
        <div class="flex items-end gap-2">
            <div :class="[
                'message-content p-3 rounded-2xl shadow-md',
                sender === 'user' ? 'bg-primary text-primary-content' : 'bg-base-200 text-base-content'
            ]">
                <div v-if="text" class="text-sm sm:text-base leading-relaxed whitespace-pre-wrap">{{ text }}</div>
                <div v-if="images && images.length" class="mt-3 flex flex-wrap gap-2">
                    <img v-for="image in images" :key="image.name" :src="getImageUrl(image)" :alt="`Food image: ${image.name}`"
                        loading="lazy"
                        class="w-20 h-20 sm:w-24 sm:h-24 object-cover rounded-lg shadow-md hover:scale-105 transition-transform duration-200 cursor-pointer" />
                </div>
            </div>
            <div :class="[
                'w-2 h-2 rounded-full mb-2',
                sender === 'user' ? 'bg-primary' : 'bg-base-200'
            ]"></div>
        </div>
        <div :class="[
            'text-xs mt-1 px-2',
            sender === 'user' ? 'text-right text-base-content/70' : 'text-left text-base-content/70'
        ]">
            {{ sender === 'user' ? 'You' : 'Krave AI' }}
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

<style scoped>
.animate-slide-up {
    animation: slideUp 0.3s ease-out;
}

@keyframes slideUp {
    from {
        opacity: 0;
        transform: translateY(20px);
    }

    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.message-content {
    position: relative;
    max-width: 100%;
    word-wrap: break-word;
}
</style>