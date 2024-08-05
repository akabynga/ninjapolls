<script>

    import Button from "../shared/Button.svelte";
    import {createEventDispatcher} from "svelte";
    import PollStore from "../stores/PollStore.js";

    let dispatcher = createEventDispatcher();
    let fields = {
        question: '',
        answerA: '',
        answerB: ''
    };
    let errors = {
        question: '',
        answerA: '',
        answerB: ''
    };
    let valid = false;
    const handleSubmit = () => {
        valid = true;
        if (fields.question.trim().length < 5) {
            valid = false;
            errors.question = 'Please enter at least 5 characters';
        } else {
            errors.question = '';
        }
        if (fields.answerA.trim().length < 1) {
            valid = false;
            errors.answerA = 'Answer can not be empty';
        } else {
            errors.answerA = '';
        }
        if (fields.answerB.trim().length < 1) {
            valid = false;
            errors.answerB = 'Answer can not be empty';
        } else {
            errors.answerB = '';
        }
        if (valid) {
            let poll = {...fields, votesA: 0, votesB: 0, id: Math.random()};
            PollStore.update(currentPolls => {
                return [poll, ...currentPolls];
            });
            dispatcher("add");
        }
        console.log(fields)
    };
</script>

<form on:submit|preventDefault={handleSubmit}>
    <div class="form-field">
        <label for="question">Poll Question:</label>
        <input id="question" type="text" bind:value={fields.question}/>
        <div class="error">{errors.question}</div>
    </div>
    <div class="form-field">
        <label for="answer-a">Answer A:</label>
        <input id="answer-a" type="text" bind:value={fields.answerA}/>
        <div class="error">{errors.answerA}</div>
    </div>
    <div class="form-field">
        <label for="answer-b">Answer B:</label>
        <input id="answer-b" type="text" bind:value={fields.answerB}/>
        <div class="error">{errors.answerB}</div>
    </div>
    <Button type="secondary" flat="true">Add Poll</Button>
</form>
<style>
    form {
        width: 400px;
        margin: 0 auto;
        text-align: center;
    }

    .form-field {
        margin: 18px;
    }

    input {
        width: 100%;
        border-radius: 6px;
    }

    label {
        text-align: left;
        margin: 10px auto;
    }

    .error {
        font-size: 12px;
        font-weight: bold;
        color: #d91b42;
    }
</style>