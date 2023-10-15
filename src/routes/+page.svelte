<script>
    import Header from "../lib/Header.svelte";
</script>

<svelte:head>
    <title>API Tester</title> 
</svelte:head>

<Header />

<div>
    <h3>Welcome to API Tester 😊</h3>
    <p>This is an amazing app to test API for <code>Get</code> and <code>Post</code> Request</p>
</div>

<style>
    div{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: calc(100vh - 30vh);
    }
</style>