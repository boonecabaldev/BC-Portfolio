<!--

Step Two – Adding Navigation Bar and Cards
 
-->

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
	
				<!-- Accordion -->
				<div class="accordion" id="accordionExampleY">
					<div class="accordion-item">
						<h2 class="accordion-header" id="headingOneY">
							<button data-mdb-collapse-init class="accordion-button" type="button" data-mdb-toggle="collapse"
								data-mdb-target="#collapseOneY" aria-expanded="true" aria-controls="collapseOneY">
								<i class="fas fa-question-circle fa-sm me-2 opacity-70"></i>Accordion Item #1
							</button>
						</h2>
						<div id="collapseOneY" class="accordion-collapse collapse show" aria-labelledby="headingOneY"
							data-mdb-parent="#accordionExampleY">
							<div class="accordion-body">
								<strong>This is the first item's accordion body.</strong> It is hidden by
								default, until the collapse plugin adds the appropriate classes that we use to
								style each element. These classes control the overall appearance, as well as
								the showing and hiding via CSS transitions. You can modify any of this with
								custom CSS or overriding our default variables. It's also worth noting that
								just about any HTML can go within the <code>.accordion-body</code>, though the
								transition does limit overflow.
							</div>
						</div>
					</div>
					<div class="accordion-item">
						<h2 class="accordion-header" id="headingTwoY">
							<button data-mdb-collapse-init class="accordion-button collapsed" type="button" data-mdb-toggle="collapse"
								data-mdb-target="#collapseTwoY" aria-expanded="false" aria-controls="collapseTwoY">
								<i class="fas fa-question-circle fa-sm me-2 opacity-70"></i>Accordion Item #2
							</button>
						</h2>
						<div id="collapseTwoY" class="accordion-collapse collapse" aria-labelledby="headingTwoY"
							data-mdb-parent="#accordionExampleY">
							<div class="accordion-body">
								<strong>This is the second item's accordion body.</strong> It is hidden by
								default, until the collapse plugin adds the appropriate classes that we use to
								style each element. These classes control the overall appearance, as well as
								the showing and hiding via CSS transitions. You can modify any of this with
								custom CSS or overriding our default variables. It's also worth noting that
								just about any HTML can go within the <code>.accordion-body</code>, though the
								transition does limit overflow.
							</div>
						</div>
					</div>
					<div class="accordion-item">
						<h2 class="accordion-header" id="headingThreeY">
							<button data-mdb-collapse-init class="accordion-button collapsed" type="button" data-mdb-toggle="collapse"
								data-mdb-target="#collapseThreeY" aria-expanded="false" aria-controls="collapseThreeY">
								<i class="fas fa-question-circle fa-sm me-2 opacity-70"></i>Accordion Item #3
							</button>
						</h2>
						<div id="collapseThreeY" class="accordion-collapse collapse" aria-labelledby="headingThreeY"
							data-mdb-parent="#accordionExampleY">
							<div class="accordion-body">
								<strong>This is the third item's accordion body.</strong> It is hidden by
								default, until the collapse plugin adds the appropriate classes that we use to
								style each element. These classes control the overall appearance, as well as
								the showing and hiding via CSS transitions. You can modify any of this with
								custom CSS or overriding our default variables. It's also worth noting that
								just about any HTML can go within the <code>.accordion-body</code>, though the
								transition does limit overflow.
							</div>
						</div>
					</div>
				</div>
				<!-- Accordion -->
				
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
