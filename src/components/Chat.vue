<template>
    <v-sheet
        class="mx-auto mt-1 container"
        elevation="4"
        max-width="85%"
    >
    <Message v-for="(message, index) in messages" :key="index" :msg="message" />
    </v-sheet>
    <v-sheet
        class="mx-auto mt-1 container pa-5"
        elevation="4"
        max-width="85%"
        height="auto"
    >
        <div class="d-flex">
            <v-textarea
                label="prompt"
                variant="underlined"
                class="me-5"
                v-model="message"
            ></v-textarea>
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
import {reactive, ref} from 'vue'
import openai from '../helpers/useOpenAi'

export default {
    components: {
        Message
    },
    setup() {
        const message = ref(null)
        const messages =  reactive([])

        const userInput = () => {
            messages.push({
                role: 'user',
                content: message.value
            })
            message.value = null
            chat(messages)
        }

        const chat = async (msgs) => {
            const chatCompletion = await openai.chat.completions.create({
                messages: msgs,
                model: 'gpt-3.5-turbo',
                stream: true
            })

            // console.log(chatCompletion.choices[0].message)
            // messages.value.push(chatCompletion.choices[0].message)

            for await (const chunk of chatCompletion) {
                // console.log(chunk.choices[0].delta.content);
                // messages.value.push({
                //     role: 'assistant',
                //     content: chunk.choices[0].delta.content
                // })

                if(chunk.choices) {

                    if(messages[messages.length - 1].role === 'assistant') {
                        messages[messages.length -1].content += chunk.choices[0].delta.content
                    } else {
                        messages.push({
                            role: 'assistant',
                            content: chunk.choices[0].delta.content
                        })
                    }

                }

            }

        }

        return {userInput, message, messages}
    },
}
</script>

<style scoped>
    .container {
        max-height: 75hv;
        overflow-y: auto;
        flex-direction: column;
    }
</style>
