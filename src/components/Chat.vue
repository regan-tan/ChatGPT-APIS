<template>
    <v-sheet
        class="mx-auto mt-1 container"
        elevation="4"
        max-width="85%"
    >
        <!-- Display chat messages -->
        <Message v-for="(message, index) in messages" :key="index" :msg="message" />
    </v-sheet>
    <v-sheet
        class="mx-auto mt-1 container pa-5"
        elevation="4"
        max-width="85%"
        style="max-height: 75vh; overflow-y: auto;"
    >
        <div class="d-flex">
            <!-- Input text area for the prompt -->
            <v-textarea
                label="Prompt"
                variant="underlined"
                class="me-5"
                v-model="message"
            ></v-textarea>
            <!-- Send button -->
            <v-btn
                @click="userInput"
                icon="mdi-send"
                class="align-self-center"
            ></v-btn>
        </div>
    </v-sheet>
</template>

<script>
import Message from './Message.vue'
import { reactive, ref } from 'vue'
import openai from '../helpers/useOpenAi'

export default {
    components: {
        Message
    },
    setup() {
        const message = ref(null)  // Holds user input message
        const messages = reactive([])  // Stores chat messages

        // Handle user input
        const userInput = () => {
            if (!message.value || message.value.trim() === '') return;  // Check for empty input

            // Add user's message to chat
            messages.push({
                role: 'user',
                content: message.value
            })
            message.value = null  // Reset input
            chat(messages)  // Call the chat function
        }

        // Chat with OpenAI API
        const chat = async (msgs) => {
            try {
                // Call OpenAI API for chat completion (replace with valid model)
                const chatCompletion = await openai.chat.completions.create({
                    messages: msgs,
                    // model: 'gpt-3.5-turbo',  // Correct model name
                    model: 'gpt-4o-mini',
                    stream: true
                })

                // Process stream response
                for await (const chunk of chatCompletion) {
                    if (chunk.choices) {
                        const lastMessage = messages[messages.length - 1]

                        // Append to the assistant's response if it's continuing
                        if (lastMessage && lastMessage.role === 'assistant') {
                            lastMessage.content += chunk.choices[0].delta.content
                        } else {
                            // Otherwise, create a new assistant message
                            messages.push({
                                role: 'assistant',
                                content: chunk.choices[0].delta.content
                            })
                        }
                    }
                }
            } catch (error) {
                console.error("Error fetching chat response:", error)
            }
        }

        return { userInput, message, messages }
    },
}
</script>

<style scoped>
.container {
    max-height: 75vh;  /* Fix typo */
    overflow-y: auto;  /* Ensure scrolling for the messages */
    flex-direction: column;
}
</style>
