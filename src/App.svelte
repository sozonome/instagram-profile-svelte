<script>
	import Profile from './Profile.svelte';
	// import InstagramLogo from './img/instagramlogo.png';
	export let name;

	let username="Profile";

	let show = false;
	let fetching = false;
	let failed = false;

	let account; 

	function updateAccount(accountName){
		username = accountName;
	}

	function fetchAccount(){
		fetching = true;
		show=false;
		failed=false;

		fetch(`https://www.instagram.com/${username}/?__a=1`)
			.then(response=> response.json())
			.then(data=>{
				if(data.error){
					console.log("Error")
				}else{
					const user = data.graphql.user
					account = {
						name: user.full_name,
						bio: user.biography,
						link: user.external_url,
						profilePicture: user.profile_pic_url,
						followers: user.edge_followed_by.count,
						following: user.edge_follow.count,
						posts: user.edge_owner_to_timeline_media.edges
					}
				}
				console.log(account, data)
			}).then(()=>{
				show=true;
			}).catch((error)=>{
				failed=true
				fetching=false;
			})
	}
</script>

<header>
	<h1>
	<img src={'/favicon.png'} alt="">
	Instagram profile</h1>
</header>

<main>
	<h1>{show ? "Found!" : `Search ${username}!` }</h1>
		<input on:input={(e)=>updateAccount(e.target.value)} type="text" placeholder="instagram username" />
		<button 
			type="submit" 
			on:click={()=> fetchAccount() }
		>Show Me</button>	
</main>

<Profile 
	fetching={fetching} 
	show={show} 
	account={account} 
	failed={failed}
/>

<footer>
	2020 - <a href="https://sznm.dev" target="_blank" rel="noopener noreferrer">SZNM</a>
</footer>

<style type="text/scss">
	main{
		width: 100%;
	}

	header{
		height: 40px;
		width: 100%;
	}
	header h1{
		padding-top:10px;
	}

	header img{
		width:30px;
		margin:0;
	}

	footer{
		align-self: flex-end;
		width: 100%;
		padding: 12px 0;
	}
</style>