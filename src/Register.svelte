<svelte:head>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</svelte:head>


<script>
    let username = ""
    let password = ""
    let email = ""
    let phone_number = ""

    let formInfo = ""

    class User {
        constructor(username, email, phone_number, password) {
            this.username = username;
            this.email = email;
            this.phone_number = phone_number;
            this.password = password;
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

        getPassword() {
            return this.password;
        }
    }

  

    function register() {
        let user = new User(username, email, phone_number, password);

        fetch("https://stanbic-poc.herokuapp.com/api/users", {
            method: 'POST',
            headers: {
                "Content-Type": "application/json"

            },
            body: JSON.stringify(user)
        }).then((response) => {
            formInfo = "User created"
        }).catch((err) => {
            formInfo = err
        })
    }

</script>

<main>
    <form on:submit|preventDefault={register}>
            <h1>👤</h1>
            <span bind:textContent={formInfo} contenteditable="false" id="formInfo"></span>
            <label class="label" for="username">Username</label>
            <input type="text" placeholder="Enter username" id="username" bind:value={username} name="username" required />

            <label class="label" for="email">Email</label>
            <input type="text" placeholder="johndoe@example.com" id="email" bind:value={email} name="email" required />

            <label class="label" for="phoneNumber">phone Number</label>
            <input type="text" placeholder="johndoe@example.com" id="phoneNumber" bind:value={phone_number} name="phoneNumber" required />

            <label class="label" for="password">Password</label>
            <input type="password" id="password" placeholder="Enter Password" bind:value={password} required />
            
            <button type="submit" value="Sign up">
                                  
                                        Sign up
                                   
            </button>
        
            <small><i><a href="/">click to login</a></i></small>
    </form>
</main>

<style>

    #formInfo {
        color: green;
        font-size: small;
        font-style: italic;
    }

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
        height: 600px;
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