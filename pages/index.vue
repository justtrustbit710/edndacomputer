<template>
  <div class="px-4 pt-12 pb-2 mx-auto sm:max-w-xl md:max-w-full lg:max-w-screen-xl md:px-24 lg:px-8 lg:py-16">
    <p class="mb-2 text-xs font-semibold tracking-wide text-gray-600 uppercase text-right pr-8">Dec 2021</p>
    <div class="max-w-xl mb-5 md:mx-auto text-center lg:max-w-2xl">
      <div class="mb-0">
        <NuxtLink
          to="/"
          aria-label="Article"
          class="text-center link-hero text-lg md:text-2xl lg:text-4xl text-purple-600"
          >edndacomputer.netlify.com</NuxtLink
        >
      </div>
      <p class="text-xs text-gray-700 md:text-lg">things that ed think are cool, even if there aren't</p>
    </div>
    <div class="my-10 text-center">
      <NuxtLink to="/" aria-label="Author" class="inline-block mb-4">
        <img alt="avatar" src="~static/img/ed_self.png" class="object-cover w-16 h-16 rounded-full shadow-sm" />
      </NuxtLink>
      <div>
        <NuxtLink to="/" aria-label="Author" class="link font-bold pb-2 text-lg text-purple-600">ed</NuxtLink>
        <p class="text-xs font-thin leading-4 text-gray-600">edndacomputer</p>
      </div>
    </div>

    <div class="mt-4">
      <h3 class="max-w-5xl mx-auto text-lg md:text-xl lg:text-2xl">Latest blog post</h3>
      <posts post-type="blog" :amount="1" />
    </div>
  </div>
</template>
