<script lang="ts">
	import favicon from "$lib/assets/favicon.svg";
	import AnimatedLogo from "$lib/home/AnimatedLogo.svelte";

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<div class="layout">
	<header>
		<AnimatedLogo />
	</header>
	<nav>
		<ul>
			<li>
				<a href="/">Home</a>
			</li>
			<li>
				<a href="/posts">Blogposts</a>
			</li>
			<li>
				<a href="/">Projects</a>
			</li>
		</ul>
	</nav>
	<main>
		{@render children()}
	</main>
	<footer>
		<a href="#">Visit the Guestbook!</a>
		<p><a href="#">(source)</a></p>
	</footer>
</div>

<style>
	:global(*) {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	a {
		color: white;
	}

	.layout {
		width: 100vw;
		height: 100vh;
	}

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	header,
	nav,
	footer {
		background-color: black;
		color: white;
	}

	header {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;

		border-bottom: 4px groove white;
	}

	nav {
		border-right: 4px groove white;

		& ul {
			display: flex;
			flex-direction: column;
			padding-left: 3rem;
		}

		& ul li {
			list-style: "(*) ";
		}
	}

	footer {
		border-top: 4px groove white;
	}

	main {
		background-color: gray;
		color: black;
		width: 100%;
		height: 100%;
	}

	@media screen and (min-width: 0px) {
		.layout {
			display: grid;
			grid-template-columns: 300px 4fr;
			grid-template-rows: 150px repeat(3, 1fr) 100px;
			grid-column-gap: 0px;
			grid-row-gap: 0px;
		}


		header { grid-area: 1 / 1 / 2 / 3; }
		nav { grid-area: 2 / 1 / 5 / 2; }
		footer { grid-area: 5 / 1 / 6 / 3; }
		main { grid-area: 2 / 2 / 5 / 3; } 

	}

	@media screen and (max-width: 500px) {
		.layout {
			display: grid;
			grid-template-columns: 1fr;
			grid-template-rows: 150px min-content 1fr 100px;
			grid-column-gap: 0px;
			grid-row-gap: 0px;
		}

		header, nav, footer, main { grid-area: unset;}

		header {
			grid-row: 1;
			grid-column: 1;
		}
		nav {
			grid-row: 2;
			grid-column: 1;
			
			height: 0px;
		}
		footer {
			grid-row: 4;
			grid-column: 1;
		}
		main {
			grid-row: 3;
			grid-column: 1;
		}
	}
</style>
