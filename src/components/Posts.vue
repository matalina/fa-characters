<template>
  <div class="mx-auto">
    <div class="position-fixed w-100 text-right p-2" style="z-index:1000">
        <button class="btn btn-dark" @click="close()">
          <i class="far fa-fw fa-times-circle"></i>
        </button>
    </div>
    <div class="bg-dark my-0 col-lg-6" >
      <article v-for="post in posts"
               class="card border-light bg-dark text-light my-2"
      >
        <header class="card-header bg-light text-dark border-light">
          By: {{ post.username }}
        </header>
        <main class="card-body border-light">
          <p v-html="output(post.message)"></p>
        </main>
      </article>
    </div>
  </div>
</template>

<script>
    import BBCode from '../functions/bbcode';

    String.prototype.replaceAll = function(search, replace) {
        //if replace is not sent, return original string otherwise it will
        //replace search string with 'undefined'.
        if (replace === undefined) {
            return this.toString();
        }

        return this.replace(new RegExp('[' + search + ']', 'g'), replace);
    };

    export default {
        name: 'posts',
        props: ['posts'],
        methods: {
            output(string) {
                string = BBCode.parse(string);
                string = string.replaceAll("\n\n",'</p><p>');
                return string;
            },
          close() {
            this.$events.fire('close','posts');
          }
        }
    }
</script>

<style>

</style>
