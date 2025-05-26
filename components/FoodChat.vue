<template>
    <div class="bg-base-100 rounded-lg border border-base-200 p-4 max-w-2xl mx-auto h-[380px] sm:h-[480px] flex flex-col">
        <div v-if="isLoading" class="absolute inset-0 flex items-center justify-center bg-base-100/80 rounded-lg z-10">
            <span class="loading loading-spinner loading-md text-primary"></span>
        </div>
        <div class="messages chat-start flex-1 overflow-y-auto space-y-4" role="log" aria-live="polite">
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
.messages::-webkit-scrollbar {
    width: 4px;
}

.messages::-webkit-scrollbar-thumb {
    background: var(--base-300);
    border-radius: 2px;
}

.messages::-webkit-scrollbar-track {
    background: transparent;
}
</style>