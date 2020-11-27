<svelte:head>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</svelte:head>

<script>
     
    // import { Router, Route, Link } from "svelte-routing";
    // import Register  from './Register.svelte';

    
   
    let username = "";
    let password = "";
    let isLoading = false;
    let formInfo = "";
    let successLogin = false
    let token = "";


    class LoginDTO {
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

    // User
    class User {
        constructor(username, email, phone_number, balance) {
            this.username = username;
            this.email = email;
            this.phone_number = phone_number;
            this.balance = balance;
        }

        getUsername() {
            return this.username
        }

        getPhoneNumber() {
            return this.phone_number
        }

        getEmail() {
            return this.email;
        }

        getBalance() {
            return this.balance;
        }
    }

    let user;


    function login() {
        isLoading = true;
        formInfo = ""

        if (username.length === 0 || password.length === 0) {
            isLoading = false
            exit
        }

        let auth_url = "http://localhost:9000/login"
        let login = new LoginDTO(username, password);

        fetch(auth_url, {
            method: 'POST',
            headers: {
                "Content-Type": "application/json"

            },
            body: JSON.stringify(login)
        }).then((response) => {
            response.headers.forEach(v => {
                        console.log(v)
            })
                    // console.log(token)
                    isLoading = false;
                    successLogin = true
                    // save token to local storage
                    localStorage.setItem("token", token)
        });

    }

    function fetch_user() {
        fetch("http://localhost:9000/api/users", {
            method: 'GET',
            headers: {
                'Authorization': 'Bearer ' + localStorage.getItem("token")
            }
        }).then((response) => {
            if (response.status === 200) {
                response.json().then((data) => {
                    user = new User(data.username, data.email, data.phoneNumber, data.balance)
                    console.log("user = " + user)
                })
            } else {
                window.location.href = "/"
            }
        })
    }

    function logout() {
        localStorage.removeItem("token");
        sessionStorage.clear()
        window.location.href = "/";
    }

</script>

<main>
    
    <form on:submit|preventDefault={login}>
        {#if successLogin}
            <div id="success" on:load={fetch_user}> 
            🔓
            <br />
            You've been successfully signed in.
            <button on:click={logout} class="btn btn-secondary">Sign out</button>
            </div>
        {:else}
            <h1>👤</h1>
            <span bind:textContent={formInfo} contenteditable="false" id="formInfo"></span>
            <label class="label" for="username">Username</label>
            <input type="text" placeholder="Enter username" id="username" bind:value={username} name="username" required />

            <label class="label" for="password">Password</label>
            <input type="password" id="password" placeholder="Enter Password" bind:value={password} required />
            <button type="submit" value="Sign in">
                                    {#if isLoading}
                                        Signing in.....
                                    {:else}
                                        Sign in 🔒
                                    {/if}
            </button>
        
            <small><i> <a href="/register">create account</a> | <a href="https://epic-babbage-574a77.netlify.app/">Sign in with SSO</a></i></small>
         
        {/if}
    </form>
</main>

<style>

    #success {
       text-align: center;
       text-rendering: optimizeLegibility;
       text-decoration-style: solid;
       font-size: x-large;
       font-family: cursive;
       font-style: oblique;
       font-stretch: condensed;
    }
    
    #formInfo {
        color: red;
        font-size: small;
        font-style: italic;
    }
    
    main {
        text-align: left;
        padding: 1em;
        max-width: 240px;
        margin: 0 auto;
    }
    
    @media (min-width: 640px) {
        main {
            max-width: none;
        }
    }
    
    .label {
        font-family: cursive;
        font-size: small;
        font-weight: 300;
        font-style: oblique;
        color:darkslategrey;
    }
    
    form {
        background: aliceblue;
        padding: 50px;
        width: 350px;
        height: 400px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        box-shadow: 0px 20px 14px 8px rgba(0, 0, 0, 0.58);
    }
    
    label {
        margin: 10px 0;
        align-self: flex-start;
        font-weight: 500;
    }
    
    input {
        border: none;
        border-bottom: 1px solid #ccc;
        margin-bottom: 20px;
        transition: all 300ms ease-in-out;
        width: 100%;
    }
    
    input:focus {
        outline: 0;
        border-bottom: 1px solid #666;
    }
    
    button {
        margin-top: 20px;
        background: darkblue;
        color: white;
        padding: 10px 0;
        width: 200px;
        border-radius: 25px;
        text-transform: uppercase;
        font-weight: bold;
        cursor: pointer;
        transition: all 300ms ease-in-out;
    }
    
    button:hover {
        transform: translateY(-2.5px);
        box-shadow: 0px 1px 10px 0px rgba(0, 0, 0, 0.58);
    }
    
    h1 {
        margin: 10px 20px 30px 20px;
        font-size: 40px;
    }
</style>