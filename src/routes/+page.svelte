<script lang="ts">
import { PUBLIC_OPENAIKEY } from '$env/static/public';
import { Configuration, OpenAIApi } from 'openai';

let question = 'I want you to act as a philosophy teacher. I will provide some topics related to the study of philosophy, and it will be your job to explain these concepts in an easy-to-understand manner. This could include providing examples, posing questions or breaking down complex ideas into smaller pieces that are easier to comprehend. My first request is "I need help understanding how different philosophical theories can be applied in everyday life."'
let results: string = ''

const configuration = new Configuration({
  apiKey: PUBLIC_OPENAIKEY,
});

const openai = new OpenAIApi(configuration);

async function handleSearch(question:string) {

    const textResponse = await openai.createChatCompletion({
            model: "gpt-3.5-turbo",
            stream: false,
            max_tokens: 2000,
            user: 'sanju',
            messages: [
                {
                    role: 'user',
                    content: question
                }
            ]
        });
        

        console.log('Results:', textResponse.data.choices[0].message);
        results = textResponse.data.choices[0].message ?  textResponse.data.choices[0].message.content : 'no results found!'

}


</script>

<h1>Welcome to I'm Philosophy Teacher</h1>

<div>
    <textarea bind:value={question} name="question" id="question" rows="10"></textarea>
    <button on:click={() => {
        handleSearch(question)        
    }}>Generate ideas</button>
</div>

<div>{results}</div>
