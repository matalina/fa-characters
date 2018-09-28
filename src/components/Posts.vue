<template>
    <div class="col-lg-6">
        <article v-for="post in posts">
            <header>
                By: {{ post.username }}
            </header>
            <main>
                <p v-html="output(post.message)"></p>
            </main>
        </article>
    </div>
</template>

<script>
    import BBCode from '../functions/bbcode';

    String.prototype.replaceAll = function(search, replace)
    {
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
            }
        }
    }
</script>

<style>

</style>