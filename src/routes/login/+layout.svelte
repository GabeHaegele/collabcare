<script lang="ts">
    import { page } from "$app/stores";
    import AnimatedRoute from "$lib/components/AnimatedRoute.svelte";
  </script>
  
  <nav class="flex justify-center my-6">
    <ul class="steps">
      <a href="/login" class="step step-primary">Login</a>
      <a
        href="/login/answer"
        class="step"
        class:step-primary={$page.route.id?.match(/answer|match/g)}>
        Answer
      </a>
      <a
        href="/login/match"
        class="step"
        class:step-primary={$page.route.id?.includes("match")}>
        Match!
      </a>
    </ul>
  </nav>
  
  <AnimatedRoute>
    <main class="card w-4/6 bg-neutral text-neutral-content mx-auto">
        <div class="card-body items-center text-center">
          <slot />
        </div>
      </main>
  </AnimatedRoute>