<script context="module">
  const allPosts = import.meta.glob('./*.{md,svx}')

  let frontMatter = []
  for (let path in allPosts) {
    frontMatter.push(
      allPosts[path]().then(({ metadata }) => {
        return { path, metadata }
      })
    )
  }

  export const load = async () => {
    const posts = await Promise.all(frontMatter)

    return {
      props: {
        posts,
      },
    }
  }
</script>

<script>
  export let posts = []

  const postsSortedByDate = posts
    .slice()
    .sort((a, b) => new Date(b.metadata.date) - new Date(a.metadata.date))

  const dateOptions = {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric',
  }
</script>

<h1>Blog</h1>
<ul>
  {#each postsSortedByDate as { path, metadata: { title, tags, date } }}
    <li>
      <a href={`/blog/${path.replace('.md', '')}`}>{title}</a>
      <p>
        {new Date(date).toLocaleString(dateOptions)}
      </p>
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
    font-size: 0.5em;
  }
</style>
