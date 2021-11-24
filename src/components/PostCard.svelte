<script>
  export let post;
</script>

<article>
  {#if post.featuredImage}
    <a href={`/blog${post.uri}`}>
        <img src={post.featuredImage.node.sourceUrl} alt={post.featuredImage.node.altText} />
    </a>
  {/if}
  <a href={`/blog${post.uri}`}>
    <h2>{post.title}</h2>
  </a>
  <div>{@html post.excerpt}</div>
</article>

<style>
  article {
    padding: 2.5rem;
    background-color: var(--color-white);
    border-radius: var(--border-radius);
    box-shadow: var(--box-shadow-2);
  }
  article > a {
    text-decoration: none;
  }
  article h2 {
    margin-top: 0;
  }
  article img {
    max-width: 100%;
  }
  article h2 {
    margin-top: 2rem;
  }
</style>
