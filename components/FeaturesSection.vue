<template>
    <section class="py-12">
        <h2 class="text-2xl font-bold text-center mb-8">Why Choose Krave AI?</h2>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            <div v-for="feature in features" :key="feature.title" 
                class="card bg-base-200 hover:bg-base-300 transition-colors duration-300">
                <div class="card-body">
                    <h3 class="card-title text-lg font-semibold">{{ feature.title }}</h3>
                    <p class="text-base-content/80">{{ feature.description }}</p>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>
const features = [
    {
        title: 'Instant Analysis',
        description: 'Get detailed insights about any food in seconds, whether you describe it or upload a photo.'
    },
    {
        title: 'Smart Recognition',
        description: 'Our AI can identify ingredients, nutritional content, and potential allergens from images.'
    },
    {
        title: 'Helpful Suggestions',
        description: 'Receive personalized recommendations and alternatives based on your preferences.'
    }
]
</script>