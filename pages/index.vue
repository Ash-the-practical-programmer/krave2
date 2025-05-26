<template>
    <div class="min-h-screen bg-base-100 flex flex-col font-sans">
        <Header />
        <main class="flex-1 container mx-auto px-4 py-8">
            <div class="max-w-4xl mx-auto space-y-12">
                <HeroSection @submit-food="handleFoodSubmit" ref="heroRef" :isLoading="isLoading" />
                <FeaturesSection />
                <FAQSection />
            </div>
        </main>
        <Footer />
    </div>
</template>

<script setup>
const heroRef = ref(null)
const isLoading = ref(false)

const handleFoodSubmit = async ({ description, images }) => {
    isLoading.value = true
    try {
        const formData = new FormData();
        formData.append('description', description || '')

        if (images && images.length > 0) {
            for (let i = 0; i < images.length; i++) {
                formData.append('images', images[i])
            }
        }

        const response = await fetch('http://localhost:3000/api/analyze-food', {
            method: 'POST',
            body: formData,
        })

        if (response.ok) {
            const parsedResponse = await response.json();
            heroRef.value.addAiMessage(parsedResponse.analysis);
        } else {
            console.error('API request failed:', response.status, await response.text())
            heroRef.value.addAiMessage("Sorry, I couldn't analyze that. Please try again.")
        }
    } catch (error) {
        console.error('Error during fetch:', error)
        heroRef.value.addAiMessage("Sorry, I couldn't analyze that. Please try again.")
    } finally {
        isLoading.value = false
    }
}

useHead({
    title: 'Krave AI - Your Food Analysis Assistant',
    meta: [
        { name: 'description', content: 'Get instant insights about any food with Krave AI. Upload photos or describe foods to learn more about them.' }
    ]
})
</script>