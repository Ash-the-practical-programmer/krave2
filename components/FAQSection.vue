<template>
    <section class="py-12">
        <h2 class="text-2xl font-bold text-center mb-8">Frequently Asked Questions</h2>
        <div class="space-y-4">
            <div v-for="(faq, index) in faqs" :key="index" 
                class="collapse collapse-arrow bg-base-200">
                <input type="radio" :name="'faq-accordion-' + index" /> 
                <div class="collapse-title text-lg font-medium">
                    {{ faq.question }}
                </div>
                <div class="collapse-content">
                    <p class="text-base-content/80">{{ faq.answer }}</p>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>
const faqs = [
    {
        question: 'How accurate is the food analysis?',
        answer: 'Our AI model is trained on millions of food items and provides highly accurate analysis. However, for specific dietary needs, always verify with product labels.'
    },
    {
        question: 'What types of foods can I analyze?',
        answer: 'You can analyze any type of food - from packaged products to home-cooked meals. Simply describe the food or upload a clear photo.'
    },
    {
        question: 'Is my data kept private?',
        answer: 'Yes, we take privacy seriously. Your uploaded images and queries are processed securely and not stored permanently.'
    }
]
</script>