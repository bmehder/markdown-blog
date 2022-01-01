<script context="module">
  const allPosts = import.meta.glob('../blog/*.{md,svx}')

  let frontMatter = []
  for (let path in allPosts) {
    frontMatter.push(
      allPosts[path]().then(({ metadata }) => {
        return { path, metadata }
      })
    )
  }

  export const load = async page => {
    const posts = await Promise.all(frontMatter)
    const tag = page.params.tag

    const filteredPosts = posts.filter(post => post.metadata.tags.includes(tag))

    return {
      props: {
        posts: filteredPosts,
        tag,
      },
    }
  }
</script>

<script>
  export let posts = []
  export let tag = ''
</script>

<h1>{tag}</h1>
<ul>
  {#each posts as { path, metadata: { title, tags } }}
    <li>
      <a href={`/tags/${path.replace('.md', '')}`}>{title}</a>
      <p>
        {#each tags as tag}
          <span><a href={`/tags/${tag}`}>#{tag}</a> </span>
        {/each}
      </p>
    </li>
  {/each}
</ul>

<style>
  ul {
    font-size: 1.5rem;
  }
  li {
    margin-bottom: 1rem;
  }
  p {
    margin: 0.25 0;
    font-size: 0.8rem;
  }
</style>
