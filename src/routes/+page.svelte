<script lang="ts">
    import type { PageData } from './$types';
    
    export let data: PageData;
</script>

<head>
  	<meta charset="utf-8">
  	<title>Collaborithm Home</title>

  	<meta name="viewport" content="width=device-width, initial-scale=1">

</head>

<body class="m-auto space-y-9 text-center">
    <header>
        <h1 class="text-4xl">Welcome to Collaborithm demo</h1>
    </header>
    <nav>
        <a href="/login"  class="underline font-semibold">Try it out.</a>
    </nav>
    
    <main class="space-y-9">
        <h2 class="text-2xl">Obtain optimal matches for your APP's and Physicians</h2>
        
        <p>Collaborithm uses the right questions along with an advanced weighting algorithm to pair Physicians and Advanced Practice Providers.</p>
    </main>
    <footer>
        <small><i>Demo: Gabe Haegele, Connor Hallemen, and David Kephart</i></small>
        <br>
        <small><a href="mailto:gjhcnp@umsystem.edu">gjhcnp@umsystem.edu</a></small>
    </footer>
</body>