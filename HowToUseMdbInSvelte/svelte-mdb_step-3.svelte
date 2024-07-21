<script>
	export const ButtonTypes = {
	  PANE_1: 'Pane 1',
	  PANE_2: 'Pane 2',
	  PANE_3: 'Pane 3'
	};
	let mode = ButtonTypes.PANE_1;
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
	<header>
		
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
	<main class="container-fluid ms-2 ms-sm-2 ms-md-3 ms-lg-4 mt-2 mt-sm-2 mt-md-3 mt-lg-4">
		
		{#if mode === ButtonTypes.PANE_1}
			<!-- panel 1 -->
		
			<!-- Some content -->
			<section class="container pt-5">
	
				<!-- Quote card -->
				<div class="card">
				  <div class="card-header">Quote</div>
				  <div class="card-body">
				    <blockquote class="blockquote mb-0">
				      <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer posuere erat a ante.</p>
				      <footer class="blockquote-footer">Someone famous in <cite title="Source Title">Source Title</cite></footer>
				    </blockquote>
				  </div>
				</div>
				<!-- Quote card -->
				
			</section>
			<!-- Some content -->
			
		{:else if mode === ButtonTypes.PANE_2}
			
			<!-- panel 2 -->
			<h2>Panel 2</h2>
			
		{:else if mode === ButtonTypes.PANE_3}
			 
			<!-- panel 3 -->
			<h2>Panel 3</h2>
			
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
	
	/* Scripts */
	
</style>
