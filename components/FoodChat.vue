<template>
    <div
        class="chat-container relative bg-base-100/95 backdrop-blur-sm rounded-2xl shadow-xl border border-base-200/50 p-4 sm:p-6 max-w-2xl mx-auto h-[380px] sm:h-[480px] md:h-[520px] flex flex-col transition-all duration-300">
        <div v-if="isLoading"
            class="absolute inset-0 flex items-center justify-center bg-base-100/90 backdrop-blur-sm rounded-2xl z-10"
            aria-live="polite">
            <div class="flex flex-col items-center gap-3">
                <span class="loading loading-spinner loading-lg text-primary"></span>
                <span class="text-sm text-primary font-medium">Analyzing...</span>
            </div>
        </div>
        <div class="messages flex-1 overflow-y-auto pr-2 space-y-4 w-full" role="log" aria-live="polite">
            <Message class="chat-bubble" v-for="msg in messages" :key="msg.id" :sender="msg.sender" :text="msg.text" :images="msg.images" />
            <div ref="messagesEndRef"></div>
        </div>
        <InputArea class="mt-4" @submit="handleSubmit" :isLoading="isLoading" />
    </div>
</template>

<script setup>
const emit = defineEmits(['submit-food'])
defineProps({
    isLoading: Boolean,
})

const messages = ref([
    {
        id: 0,
        sender: 'ai',
        text: 'Hi! What food are you curious about today? You can describe it or upload photos.',
        images: [],
    },
])
const messagesEndRef = ref(null)

const handleSubmit = ({ description, images }) => {
    if (description.trim() || images.length) {
        messages.value.push({
            id: Date.now(),
            sender: 'user',
            text: description,
            images: images,
        })
        emit('submit-food', { description, images })
    }
}

const addAiMessage = (text) => {
    messages.value.push({
        id: Date.now(),
        sender: 'ai',
        text: text,
        images: [],
    })
}

watch(
    messages,
    () => {
        nextTick(() => {
            messagesEndRef.value?.scrollIntoView({ behavior: 'smooth' });
        })
    },
    { deep: true }
)

defineExpose({ addAiMessage });
</script>

<style scoped>
.chat-container {
    background: var(--base-100);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.messages::-webkit-scrollbar {
    width: 6px;
}

.messages::-webkit-scrollbar-thumb {
    background: var(--primary);
    border-radius: 3px;
    opacity: 0.5;
}

.messages::-webkit-scrollbar-track {
    background: transparent;
}

.messages {
    scrollbar-width: thin;
    scrollbar-color: var(--primary) transparent;
}
</style>