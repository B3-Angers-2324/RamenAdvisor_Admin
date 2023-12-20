<script>
  import Template from "../../lib/template.svelte";
  import { onMount } from "svelte";
  import { API_URL } from "../../main";
  import SHA256 from "crypto-js/sha256";

  let moderators = [];
  let error = "";

  if(localStorage.getItem('ad') !== '1'){
    window.location.href = '/home';
  }

  onMount ( async () => {
    getModerators();
  })

  const getModerators = async () => {
    const response = await fetch(`${API_URL}/admin/moderator`, {
      method: "GET",
      headers: {
        "Content-Type": "application/json",
        "Authorization": "Bearer " + localStorage.getItem("token")
      },
    });
    const data = await response.json();
    if(response.status === 200){
      moderators = data;
    }else{
      error = data.message;
    }
  }

  const handleOnAddingSubmit = async (event) => {
    event.preventDefault();
    const email = event.target.email.value;
    const password = event.target.password.value;
    let passwordHash = SHA256(password).toString();

    const dataToSend = {
      email: email,
      password: passwordHash
    }

    const response = await fetch(`${API_URL}/admin/moderator`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "Authorization": "Bearer " + localStorage.getItem("token")
      },
      body: JSON.stringify(dataToSend)
    });
    const data = await response.json();
    if(response.status === 200){
      getModerators();
      error = "";
    }else{
      error = data.message;
    }
  }

  const handleOnDelete = async (id) => {
    const response = await fetch(`${API_URL}/admin/moderator/${id}`, {
      method: "DELETE",
      headers: {
        "Content-Type": "application/json",
        "Authorization": "Bearer " + localStorage.getItem("token")
      },
    });
    const data = await response.json();
    if(response.status === 200){
      getModerators();
      error = "";
    }else{
      error = data.message;
    }
  }
</script>

<Template>
  <h1>Create Moderator</h1>
  <div id="container">
      <form action="" on:submit|preventDefault={handleOnAddingSubmit}>
          <input type="text" id="email" name="email" placeholder="Email">
          <input type="password" id="password" name="password" placeholder="Password">
          <input type="submit" value="Submit">
      </form>

      <p class="error">{error}</p>

      <div id="userContainer">
        <h1>Moderator List</h1>
          {#each moderators as moderator, i}
              <div id="user">
                  <h2>{moderator.email}</h2>
                  <button on:click={() => handleOnDelete(moderator._id)}>
                    <span class="material-symbols-rounded">delete</span>
                  </button>
              </div>
          {/each}
      </div>
  </div>
</Template>

<style lang="scss">
  .error{
    margin-top: calc(var(--spacing)/2);
    color: var(--error);
  }

  h1{
      color: var(--black);
      margin-bottom: 1em;
      padding: var(--spacing) 0 0 var(--spacing);
  }

  #container{
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      width: calc(100% - var(--spacing) * 2);
      height: fit-content;
      padding: 0 var(--spacing);

      form{
        width: 100%;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        gap: var(--spacing);

        input{
          width: 100%;
          height: 3em;
          padding: 0 calc(var(--spacing) / 2);
          border-radius: var(--radius);
          border: none;
          outline: none;
          font-size: 1em;
          color: var(--black);

          &[type="submit"]{
            width: 33%;
            height: 3em;
            padding: 0 calc(var(--spacing) / 2);
            border-radius: var(--radius);
            border: none;
            outline: none;
            font-size: 1em;
            color: var(--black);
            background-color: var(--zomp);
            cursor: pointer;
          }
        }
      }

      #userContainer{
          display: flex;
          justify-content: center;
          align-items: center;
          flex-direction: column;
          width: 100%;
          padding: calc(var(--spacing) / 2);
          gap: var(--spacing);

          h1{
            color: var(--black);
            width: 100%;
            padding: 0;
            margin: 0;
          }

          #user{
              display: flex;
              align-items: center;
              width: calc(100% - var(--spacing));
              height: 4em;
              background-color: var(--zomp);
              border-radius: var(--radius);
              padding: 0 calc(var(--spacing) / 2);
              gap: var(--spacing);
              position: relative;

              h2{
                  color: var(--black);
                  font-size: 1em;
              }

              h4{
                  color: var(--black);
                  font-size: 1em;
                  position: absolute;
                  right: 0;
                  transform: translate(calc(-50% + var(--spacing) / 2), 0);
              }

              button{
                background: transparent;
                display: flex;
                justify-content: center;
                align-items: center;
                border: none;
                color: var(--bone);
                font-size: 1.25em;
                height: 1.5em;
                width: 1.5em;
                border-radius: 50%;
                background-color: rgba(0, 0, 0, 0.15);
                cursor: pointer;
                padding: 1em;
                position: absolute;
                right: calc(var(--spacing) / 2);
                transition: .5s;
                
                &:hover{
                  background-color: rgba(0, 0, 0, 0.25);
                }
              }
          }
      }
  }
</style>