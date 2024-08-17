<script>
	import 'tailwindcss/tailwind.css';
	import Navbar from './Navbar.svelte';
	import Footer from './Footer.svelte';
</script>

<Navbar />
<main class="container mx-auto flex grow">
	<slot />
</main>
<Footer />
