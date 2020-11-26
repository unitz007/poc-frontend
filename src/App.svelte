<svelte:head>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</svelte:head>

<script>

    class User {
        constructor(username, password) {
            this.username = username;
            this.password = password;
        }

        getUsername() {
            return this.username
        }

        getPassword() {
            return this.password
        }
    }

	// user login credentials from form field
	let username;
	let password;
    let user = new User(username, password);

	function login() {
        let login_url="http://localhost:9000/login"
        
        
		fetch(login_url, {
				method: 'POST',
				headers: {
					"Content-Type": "application/json"
				},
				body: JSON.stringify(user)
			}).then((response) => {
				response.json().then((data) => {
                    console.log(data.access_token)
                })
			});
			
	}
	
</script>

<main>
	<div class="container">
        <div class="row">
            <div class="col-md-4">
               &nbsp;
            </div>
            <div class="col-sm-4">
                <div id="form-container">
                    <h1 class="display-2"><b>Sign In</b></h1>
                    <form>
                        <div class="form-group">
                            <label for="username">Username</label>
                            <input type="text" placeholder="Enter username" id="username" class="form-control" name="username" bind:value={username}/>
                        </div>
                        <div class="form-group">
                            <label for="password">Password</label>
                            <input type="password" id="password" placeholder="Enter Password" class="form-control" name="password" bind:value={password} />
                        </div>
                        <p></p>
                        <div>
                            <input type="button" class="btn btn-primary" style="width: 100%" on:click={login} value="Sign in" />
                                <div align="right">
                                <small><i><a href="/register">Register</a> | <a href="https://epic-babbage-574a77.netlify.app">Login with SSO credentials</a></i></small>
                            </div>
                        </div>
                    </form>
                </div>
            </div>

        </div>
    </div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>