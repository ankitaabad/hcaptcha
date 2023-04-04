<script>
	import { browser } from "$app/environment";
 
  let token = ""
  let copied = false
  let verify_result
  function get_token(input) {
    console.log("get_token called", input)
    token= input
    navigator.clipboard.writeText(token).then(() => {
      copied = true
    })
  }
  get_token()
  if (browser) {
    window.get_token = get_token;
  }
  function verify() {
    console.log("inside verify");
    var details ={response:token,secret:"0x1f8a561c3901f0388dff37FE25B861363660AB2e"}

    var formBody = [];
    for (var property in details) {
      var encodedKey = encodeURIComponent(property);
      var encodedValue = encodeURIComponent(details[property]);
      formBody.push(encodedKey + "=" + encodedValue);
      
      let body = formBody.join("&");
      
      fetch("https://hcaptcha.com/siteverify", {
        method: "POST",
        mode: "cors",
        headers: {
          "Content-Type": "application/x-www-form-urlencoded"
        },
        body
      }).then(res => res.json()).then((result)=> {
        console.log({result})
        verify_result = result
      })
    }
  }

</script>

<svelte:head>
	<script src="https://js.hcaptcha.com/1/api.js" async defer></script>
</svelte:head>
<div class="container-md w-25 mt-4">
	<form>
		<div class="mb-3">
			<label for="exampleInputEmail1" class="form-label">Email address</label>
			<input
				type="email"
				class="form-control"
				id="exampleInputEmail1"
				aria-describedby="emailHelp"
			/>
		</div>
		<div class="mb-3">
			<label for="exampleInputPassword1" class="form-label">Password</label>
			<input type="password" class="form-control" id="exampleInputPassword1" />
		</div>
		<div class="mb-3 form-check">
			<input type="checkbox" class="form-check-input" id="exampleCheck1" />
			<label class="form-check-label" for="exampleCheck1">Check me out</label>
		</div>
		<div
			class="h-captcha"
			data-sitekey="5ce20e4e-a36c-43c9-8ee4-bd63ad94d8c7"
			data-callback="get_token"
		/>
		<div>token: {token}</div>
    <div class:visually-hidden={copied}> copied</div>
    <!-- <div>verify_result: {verify_result}</div> -->
		<input type="submit" class="btn btn-primary" value="Submit" on:click={() => verify()} />
	</form>
</div>
