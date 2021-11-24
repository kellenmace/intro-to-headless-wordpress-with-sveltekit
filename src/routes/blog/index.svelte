<script context="module">
  const query = `
    query getPosts {
      posts {
        nodes {
          databaseId
          uri
          title
          excerpt
          date
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
    }
  `;

	export async function load({ fetch }) {
    const response = await fetch(import.meta.env.VITE_PUBLIC_WORDPRESS_API_URL, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ query }),
    });

		if (response.ok) {
      const responseObj = await response.json();
      const posts = responseObj.data.posts.nodes;

			return {
				props: {
					posts
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
  import PostCard from '../../components/PostCard.svelte';
	export let posts;
</script>

<h1>Blog</h1>
{#if posts}
  <ul>
    {#each posts as post}
      <li>
        <PostCard {post} />
      </li>
    {/each}
  </ul>
{:else}
  <p>No posts found.</p>
{/if}

<style>
  ul {
    list-style: none;
    padding: 0;
  }
  ul li + li {
    margin-top: 2rem;
  }
</style>
