<template>
    <div id="app" class="container-fluid">
        <div class="row window">
            <characters :characters="characters" v-show="isActive('characters')"></characters>
            <story :story="story" :bio="bio" v-show="isActive('story')"></story>
            <posts :posts="posts" v-show="isActive('posts')"></posts>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import Characters from './components/Character';
    import Posts from './components/Posts';
    import Story from './components/Story';

    export default {
        name: 'App',
        components: {
            Characters,
            Posts,
            Story,
        },
        data () {
            return {
                characters: [],
                story: [],
                posts: [],
                bio: null,
                activePane: 'characters'
            };
        },
      methods: {
          isActive(pane) {
            return this.activePane === pane;
          },
      },
        created () {
            axios.get('https://thefirstage.org/pages/characters')
                    .then((response) => {
                        this.characters = response.data;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });

            this.$events.listen('characterSelected', (id) => {
                axios.get('https://thefirstage.org/pages/character/' + id + '/story')
                        .then((response) => {
                            this.activePane = 'story';
                            this.story = response.data;
                            let characters = this.characters;
                            for(let index in characters) {
                                if(characters[index].uid === id) {
                                    this.bio = characters[index].tid;
                                }
                            }
                        })
                        .catch(function (error) {
                            console.log(error);
                        });
            });

            this.$events.listen('topicSelected', (id) => {
                axios.get('https://thefirstage.org/pages/posts/' + id)
                        .then((response) => {
                          this.activePane = 'posts';
                            this.posts = response.data;
                        })
                        .catch(function (error) {
                            console.log(error);
                        });
            });

          this.$events.listen('close', (pane) => {
           switch(pane) {
             case 'story':
               this.activePane = 'characters';
               break;
             case 'posts':
               this.activePane = 'story';
               break;
             default:
               this.activePane = 'characters';
               break;
           }
          });
        },
    }
</script>

<style lang="scss">

</style>
