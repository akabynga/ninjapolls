<script>
    import Header from "./components/Header.svelte";
    import Footer from "./components/Footer.svelte";
    import Tabs from "./shared/Tabs.svelte";
    import CreatePollForm from "./components/CreatePollForm.svelte";
    import PollList from "./components/PollList.svelte";

    let items = ["Current Polls", "Add New Poll"];
    let activeItem = "Current Polls";

    const tabChange = (event) => {
        activeItem = event.detail;
    }
    const addPoll = () => {
        activeItem = "Current Polls";
    }
</script>

<Header/>
<main>
    <Tabs {items} {activeItem} on:tabChange={tabChange}/>
    {#if activeItem === "Current Polls"}
        <PollList/>
    {:else if activeItem === "Add New Poll"}
        <CreatePollForm on:add={addPoll}/>
    {/if}
</main>
<Footer/>
<style>
    main {
        max-width: 960px;
        margin: 40px auto;
    }
</style>