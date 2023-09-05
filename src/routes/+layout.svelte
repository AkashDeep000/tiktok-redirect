<script>
  import "../app.postcss";
  import { env } from "$env/dynamic/public";
  const { PUBLIC_APP_TITLE } = env;
</script>

<title>{PUBLIC_APP_TITLE || "Shelf — what’s on yours?"}</title>
<slot />
