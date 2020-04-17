<template>
    <div>
        <div class="character-search bg-white position-fixed mx-auto container">
            <div class="col">
                <h1>Characters</h1>
                <div class="form-group mt-3">
                    <div class="input-group">
                        <input v-model="search"
                               class="form-control"
                               placeholder="Search..."
                        />
                        <div class="input-group-append"
                             v-show="search === ''"
                        >
                            <div class="input-group-text">
                                <i class="fas fa-fw fa-search"></i>
                            </div>
                        </div>
                        <div class="input-group-append"
                             v-show="search !== ''"
                             @click="search = ''"
                        >
                            <div class="input-group-text">
                                <i class="far fa-fw fa-times-circle"></i>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="character-list row">

            <div v-for="char in filteredCharacters"
                class="col-md-3 col-sm-12"
            >
                <div :class="{ 'active': isSelected(char.uid) }"
                    class="my-2 p-3 bg-white text-dark rounded"
                    @click="selectCharacter(char.uid, char.subject)"
                >
                  <div class="w-100">
                        <span class="float-left image-cropper mr-2">
                            <img :src="getAvatar(char.avatar)"
                                   class=""
                        /></span>
                        <span>{{ char.subject }}</span><br/>
                        <small>by {{ char.username }}</small>
                  </div>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
    export default {
        name: 'characters',
        props: ['characters'],
        data () {
            return {
                selected: null,
                name: null,
                search: '',
            };
        },
        computed: {
            filteredCharacters() {
                if(this.search === '') {
                    return this.characters;
                }
                return this.characters.filter((obj) => {
                    return obj.subject.match(new RegExp('^' + this.search, 'ig'));
                });
            }
        },
        methods: {
            isSelected(id) {
                return id === this.selected;
            },
            selectCharacter(id, name) {
                this.$events.fire('characterSelected',{id,name});
                this.selected = id;
                this.name = name;

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
    .character-search {
        z-index: 10;
        box-shadow: 0 8px 6px -6px black;
    }
    .character-list {
        margin-top: 8rem;
        z-index: 1
    }

    .image-cropper {
        width: 50px;
        height: 50px;
        position: relative;
        overflow: hidden;
        border-radius: 50%;
    }

    img {
        display: inline;
        margin: 0 auto;
        height: auto;
        width: 100%;
    }
</style>
