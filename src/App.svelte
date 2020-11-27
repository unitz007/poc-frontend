<script>
    import Login from "./Login.svelte";
    import page from 'page';
      import Register from './Register.svelte';
    //   import Login from './Login.svelte'

    let current = Login;

    page('/', () => (current = Login));
    page('/register', () => (current = Register));

    page.start()

  </script>
  
  <style>	
	section {
	  height: 100%;
	  width: 100%;
	  display: flex;
	  justify-content: center;
	  align-items: center;
	  background-color: rgb(214, 224, 240);	
	}
  </style>

<section>
	<svelte:component this={current}/>
  </section>