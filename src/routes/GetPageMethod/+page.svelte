<script>
    import Header from "../../lib/Header.svelte";
    import axios from 'axios'
    let RequestAPI = "";
    let ResponseTxt = ""
    let lblstatus = ""
    const requestAPI = async () => {
		try {
            if(RequestAPI != "")
            {
                const res = await axios.get(RequestAPI, {
                    headers: {},
                    params: {},
                });
                //console.log(res)
                ResponseTxt = JSON.stringify(res.data,null, 3);
                lblstatus = `Status: ${res.status}`
            }
            else{
                alert("The Request URL is empty")
            }
		} catch (err) {
			console.log(err);
            ResponseTxt = err.message
            lblstatus = `Status: ${err.response.status}`
		}
	};
    function ResetAPI(){
        ResponseTxt = ""
    }
    function CopyResponse(){
        navigator.clipboard.writeText(ResponseTxt);
        alert("Copied successfully!")
    }
</script>
<Header />
<br>
<div class="container">
    <h4>Get Method</h4>
    <br>
    <table>
        <tr>
            <td><h6>URL</h6></td>
            <td style="width: 100%;"><input type="text" class="form form-control" bind:value={RequestAPI}/></td>
        </tr>
    </table>
    <br>
    <button on:click={requestAPI} class="btn btn-success">Send</button>
    <button on:click={ResetAPI} class="btn btn-success">Reset</button>
    {#if ResponseTxt!=""}
    <button on:click={CopyResponse} class="btn btn-success">Copy Response</button>
    {/if}
    <br>
    <br>
    {#if ResponseTxt != ""}
    <p>{lblstatus}</p>
    <pre bind:innerHTML={ResponseTxt} contenteditable="false"></pre>
    {/if}
</div>
<style>
    pre{
        background-color: bisque;
        padding: 10px;
        font-weight: bold;
        font-family: Arial, Helvetica, sans-serif;
        border-radius: 10px;
        box-shadow: 0px 0px 10px 2px black;
    }
    p{
        background-color: bisque;
        border-radius: 10px;
        box-shadow: 0px 0px 10px 2px black;
        padding: 10px;
        font-weight: bold;
    }
</style>