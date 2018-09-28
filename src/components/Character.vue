<template>
    <div class="col-lg-3">
        <ul v-for="char in characters"
            class="list-group"
        >
            <li :class="{ 'active': isSelected(char.uid) }"
                class="list-group-item my-2"
                @click="selectCharacter(char.uid)"
            >
                    <span><img :src="getAvatar(char.avatar)"/></span>
                    <span>{{ char.subject }}</span><br/>
                    <span>by {{ char.username }}</span>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name: 'characters',
        props: ['characters'],
        data () {
            return {
                selected: null,
            };
        },
        methods: {
            isSelected(id) {
                return id === this.selected;
            },
            selectCharacter(id) {
                this.$events.fire('characterSelected',id);
                this.selected = id;
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
        },
        mounted() {
        }
    }
</script>

<style lang="scss">
    img {
        height: 50px;
        width: auto;
    }
</style>