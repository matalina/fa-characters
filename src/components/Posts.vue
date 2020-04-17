<template>
  <div class="mx-auto">
    <div class="position-fixed container text-right p-3" style="z-index:10000">
        <button class="btn btn-info" @click="close()">
          <i class="far fa-fw fa-times-circle"></i>
        </button>
    </div>
    <div class="bg-dark my-0" >
      <article v-for="post in posts"
               class="card border-light bg-dark text-light my-2"
      >
        <header class="card-header bg-light text-dark border-light">
          {{ title }} by {{ post.username }}
        </header>
        <main class="card-body border-light">
          <span class="float-left image-cropper mr-2">
            <img :src="getAvatar(post.character.avatar)"
                 class=""
            /></span>
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
        props: ['posts','title'],
        methods: {
            output(string) {
                string = BBCode.parse(string);
                string = string.replaceAll("\n\n",'</p><p>');
                return string;
            },
          close() {
            this.$events.fire('close','posts');
          },
          getAvatar(url) {
            if(url === '') {
              url = 'https://thefirstage.org/forums/images/default_avatar.png';
            }
            else if(url.search(/^http/) === -1) {
              url = 'https://thefirstage.org/forums/' + url;
            }
            return url;
          },
        }
    }
</script>

<style>

</style>
