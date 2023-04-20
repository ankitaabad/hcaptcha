<script>
	import { browser } from "$app/environment";
 
  let token = ""
  let copied = true
  let form;
  let verify_result
  function onSubmit(input) {
    console.log("get_token called", input)
    token= input
    navigator.clipboard.writeText(token).then(() => {
      copied = false
    })
  }
  if (browser) {
    window.onSubmit = onSubmit;
  }
  

</script>
<svelte:head>
  <script src="https://www.google.com/recaptcha/api.js"></script>
</svelte:head>

<div class="container-md w-25 mt-4">
  <div>Recaptcha V2</div>

	<form bind:this={form}>
		<div class="form-outline mb-3">
      <input
      type="email"
      class="form-control"
      id="exampleInputEmail1"
      aria-describedby="emailHelp"
			/>
			<label for="exampleInputEmail1" class="form-label">Email address</label>
		</div>
		<div class="form-outline mb-3">
      <input type="password" class="form-control" id="exampleInputPassword1" />
			<label for="exampleInputPassword1" class="form-label">Password</label>
		</div>
		
		
		<div>token: {token}</div>
    <div class:visually-hidden={copied}> copied</div>
    <!-- <div>verify_result: {verify_result}</div> -->
    <div class="g-recaptcha btn btn-primary" 
    data-sitekey="6LcCHI0iAAAAAH_-L_BW49nb1CuPiuLAzg8UIGbo" data-callback="onSubmit"
   
    ></div>
    <input class="btn btn-primary" type="submit" value="submit"/>

	</form>


</div>
