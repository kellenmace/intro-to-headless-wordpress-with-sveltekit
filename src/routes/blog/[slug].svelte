<script context="module">
  export const prerender = true;

  const query = `
    query getPostBySlug($slug: ID!) {
      post(id: $slug, idType: SLUG) {
        date
        title
        content
        author {
          node {
            name
          }
        }
        categories {
          nodes {
            name
          }
        }
        featuredImage {
          node {
            sourceUrl
            altText
            mediaDetails {
              width
              height
            }
          }
        }
      }
    }
  `;

  export async function load({ page, fetch }) {
    const response = await fetch(import.meta.env.VITE_PUBLIC_WORDPRESS_API_URL, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        query,
        variables: {
          slug: page.params.slug,
        }
      }),
    });

		if (response.ok) {
      const responseObj = await response.json();
      const { post } = responseObj.data;

			return {
				props: {
					post
				}
			};
		}

		return {
			status: response.status,
			error: new Error(`Could not load ${url}`)
		};
	}
</script>

<script>
  export let post;
  const formatDate = (date) => new Date(date).toLocaleDateString();
  const categories = post.categories?.nodes?.map(category => category.name) ?? [];
</script>

<a href="/blog" class="blog-link">&#8592; Blog</a>
<article>
  {#if post.featuredImage}
    <img src={post.featuredImage.node.sourceUrl} alt={post.featuredImage.node.altText} />
  {/if}
  <h1>{post.title}</h1>
  <p class="post-meta">
    ✍️ {post.author.node.name} on {formatDate(post.date)}
  </p>
  <div>{@html post.content}</div>
  {#if categories.length}
    <div class="category-list">
      <h4>Categorized As</h4>
      <p>{categories.join(', ')}</p>
    </div>
  {/if}
</article>

<style>
  .blog-link {
    text-decoration: none;
  }
  article {
    margin-top: 2rem;
  }
  article img {
    max-width: 100%;
  }
  .category-list {
    border-top: 2px solid var(--color-gray-9);
    margin-top: 2.5rem;
    padding-top: 2rem;
  }
  .category-list h4 {
    margin: 0;
  }
</style>
