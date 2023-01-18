<template>
  <Layout>
    <br />
    <g-link to="/" class="nes-btn is-success">Go Back</g-link>
    <div class="post-title">
      <h1>{{ $page.post.title }}</h1>
      <p class="post-date">
        {{ $page.post.author }} | {{ $page.post.date }} | {{ $page.post.timeToRead }} min read
      </p>
    </div>
    <div class="post-content">
      <p v-html="$page.post.content" />
    </div>
  </Layout>
</template>

<page-query>
query Post ($path: String!) {
   post: post (path: $path) {
    id
    title
    author
    content
    date (format: "D MMMM YYYY")
    timeToRead
  }
}
</page-query>

<style>
.post-title {
  margin-top: 20px;
  text-align: center;
  font-size: 0.6rem;
  line-height: 1.4em;
  padding: 2em 0;
}

.post-title > h1 {
  margin-bottom: 1rem;
}

.post-date {
  font-size: 16px;
  font-weight: 400;
}

.post-content {
  font-size: 20px;
}

</style>
