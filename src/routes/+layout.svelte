<script>
	import 'tailwindcss/tailwind.css';
	import Navbar from './Navbar.svelte';
	import Footer from './Footer.svelte';
	export const prerender = true;
	export const trailingSlash = 'always';
</script>

<Navbar />
<main class="container mx-auto flex grow">
	<slot />
</main>
<Footer />
