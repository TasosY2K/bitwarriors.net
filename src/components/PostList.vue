<template>
  <div class="post-list">
    <div class="nes-container with-title is-rounded post" style="padding-bottom: 40px"  v-if="$theme === 'light'">
      <p class="title">{{ post.title }}</p>
      <i class="date">{{ post.date }}</i>
      <p class="description" v-html="post.description" />
      <b>{{ post.timeToRead }} min read</b> - <b>{{ post.author }}</b>&nbsp;
      <g-link :to="post.path" class="nes-btn is-success" style="float: right;"
        >Read More
        <span class="visuallyhidden">about {{ post.title }}</span></g-link
      >
    </div>

    <div class="nes-container is-dark with-title is-rounded post" style="padding-bottom: 40px" v-if="$theme === 'dark'">
      <p class="title">{{ post.title }}</p>
      <i class="date">{{ post.date }}</i>
      <p class="description" v-html="post.description" />
      <b>{{ post.timeToRead }} min read</b> - <b>{{ post.author }}</b>&nbsp;
      <g-link :to="post.path" class="nes-btn is-success" style="float: right"
        >Read More
        <span class="visuallyhidden">about {{ post.title }}</span></g-link
      >
    </div>
    
  </div>
</template>

<script>
export default {
  props: ["post"],
};
</script>

<style>

.post-list {
  transition-duration: 0.3s;
  margin-top: 2rem;
}

.post:hover {
  transform: translateY(-2px);
}

/* .date {
  margin-bottom: 3rem;
} */

.description {
  margin-top: 0.6rem;
}

.visuallyhidden {
  position: absolute !important;
  width: 1px;
  height: 1px;
  padding: 0;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  clip-path: inset(50%);
  border: 0;
}
.visuallyhidden:focus {
  display: inline-block;
  padding: 15px 23px 14px;
  width: auto;
  height: auto;
  clip: auto;
  color: var(--text-color);
  background-color: var(--bkg-color);
  clip-path: unset;
}
</style>