<script>
  import Profile from "./Profile.svelte";

  let username="";

  let show = false;
  let fetching = false;
  let failed = false;

  let account;

  function fetchAccount() {
    fetching = true;
    show = false;
    failed = false;

    fetch(`https://www.instagram.com/${username}/?__a=1`)
      .then((response) => response.json())
      .then((data) => {
        if (data.error) {
          console.log("Error");
        } else {
          const user = data.graphql.user;
          account = {
            name: user.full_name,
            bio: user.biography,
            link: user.external_url,
            profilePicture: user.profile_pic_url,
            followers: user.edge_followed_by.count,
            following: user.edge_follow.count,
            posts: user.edge_owner_to_timeline_media.edges,
          };
        }
      })
      .then(() => {
        show = true;
      })
      .catch((error) => {
        failed = true;
      });
  }

  function handleKeyPress(event){
    if(event.key==='Enter'){
      fetchAccount()
    }
  }
</script>

<header>
  <h1>
    <img src={'/favicon.png'} alt="" />
    Instagram Profile
  </h1>
</header>

<main>
  <h1>{show ? 'Found!' : `Search ${username}!`}</h1>
  <input
    bind:value={username}
    type="text"
    placeholder="instagram username"
    on:keydown={handleKeyPress}
  />
  <button type="submit" on:click={() => fetchAccount()}>Show Me</button>
</main>

<Profile {fetching} {show} {account} {failed} />

<footer>
  2020 -
  <a href="https://sznm.dev" target="_blank" rel="noopener noreferrer">SZNM</a>
</footer>
