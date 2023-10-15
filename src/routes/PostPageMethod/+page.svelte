<script>
    import Header from "../../lib/Header.svelte";
    import axios from 'axios'
    let RequestAPI = "";
    let ResponseTxt = "";
    let APIBody = {}
    let bodyKey = ""
    let bodyvalue = ""
    let lblstatus = ""

    const requestAPI = async () => {
		try {
            if(RequestAPI != "")
            {
                axios.post(
                    RequestAPI,APIBody
                    ,
                    {
                        params: { 'api-version': '3.0' },
                        headers: {
                            'content-type': 'application/json',
                            'X-RapidAPI-Key': 'your-rapidapi-key',
                            'X-RapidAPI-Host': 'microsoft-translator-text.p.rapidapi.com',
                        },
                    }
                )
                .then(function (response) {
                    ResponseTxt = JSON.stringify(response.data,null,2)
                    lblstatus = `Status: ${response.status}`
                })
                .catch(function (error) {
                    ResponseTxt = error.message
                    lblstatus = `Status: ${error.response.status}`
                });
            }
            else{
                alert("The Request URL is empty")
            }
		} catch (err) {
            ResponseTxt = err.message
            lblstatus = `Status: ${err.response.status}`
		}
	};

    function ResetAPI(){
        ResponseTxt = ""
        APIBody = {}
    }

    function CopyResponse(){
        navigator.clipboard.writeText(ResponseTxt);
        alert("Copied successfully!")
    }

    const AddKeyValuePair = () => {
        APIBody[bodyKey] = bodyvalue
    }

    const RemoveKeyValuePair = () => {
        delete(APIBody[bodyKey]);
        APIBody = APIBody
    }
</script>
<Header />
<br>
<div class="container">
    <h4>Post Method</h4>
    <br>
    <table>
        <tr>
            <td><h6>URL</h6></td>
            <td style="width: 100%;"><input type="text" class="form form-control" bind:value={RequestAPI}/></td>
        </tr>
    </table>
    <br>
    <div>
        <table>
            <tr>
                <td><input type="text" placeholder="Key" class="form form-control" bind:value={bodyKey} disabled={RequestAPI==""?true:false}/></td>
                <td><input type="text" placeholder="value" class="form form-control" bind:value={bodyvalue} disabled={RequestAPI==""?true:false}/></td>
                <td><button class="btn btn-success" on:click={AddKeyValuePair} disabled={RequestAPI==""?true:false}>+</button></td>
                <td><button class="btn btn-success" on:click={RemoveKeyValuePair} disabled={RequestAPI==""?true:false}>-</button></td>
            </tr>
        </table>
    </div>
    <br>
    {#if JSON.stringify(APIBody,null,2) != "{}"}
    <div id="RequestBodystyle">
        <h6>Request Body</h6>
        <pre class="resstyle">{JSON.stringify(APIBody,null,2)}</pre>
    </div>
    {/if}
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
    <pre bind:innerHTML={ResponseTxt} contenteditable="false" class="resstyle"></pre>
    {/if}
</div>
<style>
    .resstyle{
        background-color: bisque;
        padding: 10px;
        font-weight: bold;
        font-family: Arial, Helvetica, sans-serif;
        border-radius: 10px;
        box-shadow: 0px 0px 10px 2px black;
    }
    #RequestBodystyle{
        display: flex;
        flex-direction: column;
    }
    p{
        background-color: bisque;
        border-radius: 10px;
        box-shadow: 0px 0px 10px 2px black;
        padding: 10px;
        font-weight: bold;
    }
</style>