<script>
  export let fetching;
  export let show;
  export let account;
  export let failed;
</script>

<div>
	{#if fetching}
		{#if !show}
			<p>Fetching...Please Wait</p>
		{/if}
		{#if show}
			<div class="profileHead">
				<img src={account.profilePicture} alt="">
				<div>
					<h3>{account.name}</h3>
					<p>
						{account.bio}
            {#if account.link!==null}
						  <a href={account.link} target="_blank" rel="noopener noreferrer">{account.link}</a>
            {/if}
          </p>
				</div>
			</div>
			<div class="profileInfo">
				<div>
					followers
					<h3>
						{account.followers}
					</h3>
				</div>
				<div>
					following
					<h3>
						{account.following}
					</h3>
				</div>
			</div>
			<h4>Recent Posts</h4>
			<div class="media">
				{#each account.posts as post, index}
					<div class="mediaContainer">
						<a href={`https://instagram.com/p/${post.node.shortcode}`} target="_blank" rel="noopener noreferrer">
							<img src={post.node.thumbnail_src} alt={post.node.accessibility_caption}/>
						</a>
					</div>
				{/each}
			</div>
		{/if}
    {#if failed}
      <p>Not Found</p>
    {/if}
	{/if}
</div>

<style>
  .profileHead, .profileInfo, .media{
		display: flex;
	}

	.profileInfo{
		text-align: center;
	}
	.profileInfo h3{
		margin: 0;
	}
	.profileInfo>div{
		flex-basis: 50%;
	}
	.profileHead img{
		height: 80px;
		padding: 10px;
		border-radius: 50%;
		margin-right: 20px; 
	}
	.media{
		flex-wrap: wrap;
	}
	.mediaContainer{
		flex-basis: 33%;
		margin:0;
	}
	.mediaContainer img{
		margin: 0;
	}
</style>