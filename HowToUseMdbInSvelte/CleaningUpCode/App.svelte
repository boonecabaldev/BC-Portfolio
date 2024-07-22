<script>
	import Panel1 from './Panel1.svelte';
	import Panel2 from './Panel2.svelte';
	import Panel3 from './Panel3.svelte';

	let activePanel = Panel1;
	
	export const ButtonTypes = {
	  PANEL_1: 'Panel 1',
	  PANEL_2: 'Panel 2',
	  PANEL_3: 'Panel 3'
	};
	let mode = ButtonTypes.PANEL_1;

	let disableMenuItem = false;

	const navbar_Click = function(e) {
		console.log(`e.target.value = ${e.target.value}`);
		mode = e.target.value;
	}
</script>

<svelte:head>
	
	<!-- Bootstrap CSS -->
	<link 
		href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" 
		rel="stylesheet" 
		integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" 
		crossorigin="anonymous"
	/>

	<!-- Font Awesome -->
	<link
	  href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css"
	  rel="stylesheet"
	/>
	
	<!-- Google Fonts -->
	<link
	  href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap"
	  rel="stylesheet"
	/>
	
	<!-- MDB -->
	<link
	  href="https://cdnjs.cloudflare.com/ajax/libs/mdb-ui-kit/7.3.2/mdb.min.css"
	  rel="stylesheet"
	/>
	
</svelte:head>

<body>

	<!-- header -->
	<header class="mb-5">
		
		<!-- navbar -->
		<nav class="navbar navbar-expand-lg navbar-light bg-body-tertiary">
		  <div class="container-fluid">
				
				<!-- Branding Logo -->
		    <a class="navbar-brand" href="navbar_Click">Deeznut</a>

				<!-- Dropdown -->
				<ul class="navbar-nav">
					<div class="nav-item dropdown pe-3">

					<!-- Menu item: dropdown button -->
					<a
						data-mdb-dropdown-init
						class="nav-link dropdown-toggle"
						id="navbarDropdownMenuLink"
						role="button"
						aria-expanded="false"
					>{mode}</a>
							
						<!-- Menu item: dropdown menu -->
					<ul class="dropdown-menu" aria-labelledby="navbarDropdownMenuLink">
						
						{#each Object.keys(ButtonTypes) as buttonTypeKey}
						<li class="dropdown-item">
							<button 
								on:click={navbar_Click}
								disabled={disableMenuItem = ButtonTypes[buttonTypeKey] === mode}
								type="button" 
								class="btn btn-link" 
								data-mdb-ripple-init
								value={ButtonTypes[buttonTypeKey]}
								>{ButtonTypes[buttonTypeKey]}</button>
						</li>
						{/each}
						
					</ul>
					<!-- Menu item: dropdown menu -->
						
					</div>
				</ul>
				<!-- Dropdown -->
	
		  </div>
		</nav>
		<!-- navbar -->
		
	</header>
	<!-- header -->

	<!-- main -->
	<main class="container py-3">
		
		{#if mode === ButtonTypes.PANEL_1}
			
			<!-- panel 1 -->
			<svelte:component this={Panel1} />
			<!-- panel 1 -->
			
		{:else if mode === ButtonTypes.PANEL_2}
			
			<!-- panel 2 -->
			<svelte:component this={Panel2} />
			<!-- panel 2 -->
			
		{:else if mode === ButtonTypes.PANEL_3}
				
			<!-- panel 3 -->
			<svelte:component this={Panel3} /> 
			<!-- panel 3 -->
			
		{/if}
		
	</main>
	<!-- main -->
	
	<!--  Bootstrap Bundle with Popper -->
	<script 
		src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" 
		integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" 
		crossorigin="anonymous"></script>
	
	<!-- MDB -->
	<script
	  type="text/javascript"
	  src="https://cdnjs.cloudflare.com/ajax/libs/mdb-ui-kit/7.3.2/mdb.umd.min.js"
	></script>
	
</body>

<style>
	
</style>
