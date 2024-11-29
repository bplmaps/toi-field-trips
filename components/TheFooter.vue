<template>
  <footer class="bg-midnight text-pewter">
    <div class="container mx-auto px-5 py-8 sm:py-14">
      <div class="flex flex-wrap justify-start items-end space-x-8 pb-6 mb-4 border-b border-pewter sm:pb-10 sm:mb-6 xl:pb-14 xl:space-x-16">
        <a href="https://www.leventhalmap.org/" target="_blank">
          <nuxt-img class="h-7 w-auto sm:h-10 xl:h-18" src="/bpl-logo.png" sizes="sm:224px md:320px lg:576px xl:576px 2xl:576px" alt="Normal V. Leventhal Map & Education Center logo" />
        </a>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  name: 'TheFooter'
}
</script>