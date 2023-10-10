# Connecting to ChatGPT APIs with VueJS and Vuetify

This repository contains the code for a tutorial on how to connect to ChatGPT APIs using VueJS and Vuetify. The tutorial provides a step-by-step guide on how to establish a connection and have interactive conversations with AI.

## Prerequisites

To follow along with this tutorial, you'll need the following:

- [Vue.js](https://vuejs.org/) installed on your machine
- Basic understanding of Vue.js and Vuetify
- [OpenAI API credentials](https://platform.openai.com/)
- [Youtube Tutorial](https://youtu.be/EqvLoC8fjzA?si=B4VNGVNECbbOoWdV)

## Installation

1. Clone this repository using the following command: 
   ```
   git clone https://github.com/sohrabq/ChatGPT-APIS.git
   ```

2. Navigate to the project directory:
   ```
   cd ChatGPT-APIS
   ```

3. Install the project dependencies:
   ```
   npm install
   ```

## Configuration

1. create a `.env` file.

2. Add `VITE_OPENAI_API_KEY` variable with your actual OpenAI API key in the following line:
   ```
   VITE_OPENAI_API_KEY={your_openai_api_key_here}
   ```
## Run
```
npm run dev
```
The project should now be up and running. You can access it by going to `http://localhost:xxxx` in your browser.

You will see a chat interface where you can have interactive conversations with the ChatGPT model. Type in your message and click send icon to send it. The assistant will respond with a generated text.

You can continue the conversation by typing in your next message and clicking send button icon.

Feel free to explore and modify the code as needed. The tutorial provides a basic implementation, but you can extend it to add more features and functionality.
