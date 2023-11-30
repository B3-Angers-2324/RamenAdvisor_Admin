<script>
  import Template from "../../lib/template.svelte";
</script>

<Template>
  <h1>Create Moderator</h1>
  <div id="container">
      <form>
          <input type="text" id="name" name="name" placeholder="Nom">
          <input type="text" id="surname" name="surname" placeholder="Prénom">
          <input type="submit" value="Submit">
      </form>

      <div id="userContainer">
        <h1>Moderator List</h1>
          {#each Array(12) as _,i}
              <div id="user">
                  <h2>Nom {i}</h2>
                  <h2>Prénom {i}</h2>
                  <button>
                    <span class="material-symbols-rounded">delete</span>
                  </button>
              </div>
          {/each}
      </div>
  </div>
</Template>

<style lang="scss">
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
          padding: var(--spacing);
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