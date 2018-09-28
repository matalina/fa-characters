<template>
    <div id="app" class="container-fluid">
        <div class="row window">
            <characters :characters="characters"></characters>
            <story :story="story"></story>
            <posts :posts="posts"></posts>
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
            };
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
                            this.story = response.data;
                        })
                        .catch(function (error) {
                            console.log(error);
                        });
            });

            this.$events.listen('topicSelected', (id) => {
                axios.get('https://thefirstage.org/pages/posts/' + id)
                        .then((response) => {
                            this.posts = response.data;
                        })
                        .catch(function (error) {
                            console.log(error);
                        });
            });
        },
    }
</script>

<style lang="scss">

</style>
