<template>
    <div class="border-t border-base-200 pt-4">
        <div v-if="selectedImages.length" class="flex gap-2 mb-4 overflow-x-auto pb-2">
            <div v-for="image in selectedImages" :key="image.name" class="relative flex-shrink-0">
                <img :src="getPreviewUrl(image)" :alt="`Preview: ${image.name}`"
                    class="w-16 h-16 object-cover rounded-md" />
                <button @click="removeImage(image)"
                    class="absolute -top-1 -right-1 bg-base-100 rounded-full w-5 h-5 flex items-center justify-center text-xs shadow-sm hover:bg-base-200"
                    aria-label="Remove image">×</button>
            </div>
        </div>
        <div v-if="errorMessage" class="text-xs text-error mb-2">{{ errorMessage }}</div>
        <div class="flex gap-2">
            <input type="text" v-model="message" placeholder="Type a message..."
                class="input input-bordered flex-1 h-10 min-h-[40px] text-sm bg-base-100"
                @keyup.enter="sendMessage" />
            <label for="image-upload" class="btn btn-square btn-sm">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24"
                    stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round"
                        d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z" />
                </svg>
            </label>
            <input type="file" id="image-upload" multiple accept="image/png,image/jpeg,image/webp"
                @change="handleImageUpload" class="hidden" />
            <button @click="sendMessage" :disabled="isLoading || (!message.trim() && !selectedImages.length)"
                class="btn btn-primary btn-square btn-sm">
                <span v-if="isLoading" class="loading loading-spinner loading-sm"></span>
                <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24"
                    stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8" />
                </svg>
            </button>
        </div>
    </div>
</template>

<script setup>
const emit = defineEmits(['submit']);
defineProps({
    isLoading: Boolean,
    showInput: {
        type: Boolean,
        default: true,
    },
});

const message = ref('');
const selectedImages = ref([]);
const previewUrls = ref({});
const errorMessage = ref('');

const handleImageUpload = (event) => {
    errorMessage.value = '';
    const files = Array.from(event.target.files);
    const maxSize = 5 * 1024 * 1024;
    const validFiles = files.filter((file) => {
        if (file.size > maxSize) {
            errorMessage.value = `File "${file.name}" is too large! Please upload images under 5MB.`;
            return false;
        }
        return true;
    });

    if (validFiles.length < files.length && !errorMessage.value) {
        errorMessage.value = 'Some files were too large and were not added.';
    }

    selectedImages.value.push(...validFiles);
    validFiles.forEach((file) => {
        const reader = new FileReader();
        reader.onload = (e) => {
            previewUrls.value[file.name] = e.target.result;
        };
        reader.readAsDataURL(file);
    });
};

const getPreviewUrl = (file) => previewUrls.value[file.name];

const removeImage = (file) => {
    selectedImages.value = selectedImages.value.filter((img) => img !== file);
    delete previewUrls.value[file.name];
    if (!selectedImages.value.length) {
        errorMessage.value = '';
    }
};

const sendMessage = () => {
    if (message.value.trim() || selectedImages.value.length) {
        emit('submit', {
            description: message.value,
            images: selectedImages.value,
        });
        message.value = '';
        selectedImages.value = [];
        previewUrls.value = {};
        errorMessage.value = '';
    }
};
</script>