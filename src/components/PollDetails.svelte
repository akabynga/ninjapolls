<script>
    import Card from "../shared/Card.svelte";
    import {createEventDispatcher} from "svelte";
    import PollStore from "../stores/PollStore.js";
    import Button from "../shared/Button.svelte";
    import {tweened} from "svelte/motion";

    export let poll = {};
    let dispatch = createEventDispatcher();
    // reactive values
    $: totalVotes = poll.votesA + poll.votesB;
    $: percentA = Math.floor(100 / totalVotes * poll.votesA) || 0;
    $: percentB = Math.floor(100 / totalVotes * poll.votesB) || 0;

    // tweened percentages
    const tweenedA = tweened(0);
    const tweenedB = tweened(0);
    $: tweenedA.set(percentA);
    $: tweenedB.set(percentB);

    const handleVote = (option, id) => {
        PollStore.update(currentPolls => {
            let upvotedPoll = currentPolls.find(poll => poll.id === id);
            if (option === "a") {
                upvotedPoll.votesA++;
            } else if (option === "b") {
                upvotedPoll.votesB++;
            }
            return currentPolls;
        });
    };

    const handleDelete = (id) => {
        PollStore.update(currentPolls => {
            return currentPolls.filter(poll => poll.id !== id);
        });
    };
</script>
<Card>
    <div class="poll">
        <h3>{poll.question}</h3>
        <p>Total votes: {totalVotes}</p>
        <div class="answer" on:click={()=> handleVote('a', poll.id)}>
            <div class="percent percent-a" style="width:{$tweenedA}%;"></div>
            <span>{poll.answerA} ({poll.votesA})</span>
        </div>
        <div class="answer" on:click={()=> handleVote('b', poll.id)}>
            <div class="percent percent-b" style="width:{$tweenedB}%;"></div>
            <span>{poll.answerB} ({poll.votesB})</span>
        </div>
        <div class="delete">
            <Button flat={true} on:click={() => handleDelete(poll.id)}>Delete</Button>
        </div>
    </div>
</Card>


<style>
    h3 {
        margin: 0 auto;
        color: #555;
    }

    p {
        margin-top: 6px;
        font-size: 14px;
        color: #aaa;
        margin-bottom: 30px;
    }

    .answer {
        user-select: none;
        background: #fafafa;
        margin: 10px auto;
        cursor: pointer;
        position: relative;
    }

    .answer:hover {
        opacity: 0.6;
    }

    span {
        display: inline-block;
        padding: 10px 20px;
    }

    .percent {
        height: 100%;
        position: absolute;
        box-sizing: border-box;
    }

    .percent-a {
        border-left: 4px solid #d91b42;
        background: rgba(217, 27, 66, 0.2);
    }

    .percent-b {
        border-left: 4px solid #45c496;
        background: rgba(69, 196, 150, 0.2);
    }

    .delete {
        margin-top: 30px;
        text-align: center;
    }
</style>