<template>
    <div class="col-lg-3">
        <div class="story-search row bg-white position-fixed mx-auto">
            <div class="col">
                <h1>Their Story</h1>
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
        <div class="story-list row">
            <div class="col">
                <ul v-for="topic in filteredStory"
                    class="list-group"
                >
                    <li :class="{ active: isSelected(topic.tid) }"
                        class="list-group-item my-2"
                        @click="selectTopic(topic.tid)"
                    >
                        {{ topic.subject}}<br/>
                        <small>
                            on
                            <template v-for="forum in topic.forum">
                                {{ forum }}
                            </template><br/>
                        </small>
                        Players:
                        <template v-for="player in topic.players">
                            {{ player }}
                        </template>

                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'story',
        props: ['story'],
        data () {
            return {
                selected: null,
                search: '',
            };
        },
        computed: {
            filteredStory() {
                if(this.search === '') {
                    return this.story;
                }
                return this.story.filter((obj) => {
                    if(obj.subject.match(new RegExp('^' + this.search, 'ig'))) {
                        return true;
                    }
                    console.log(obj);
                    for(let index in obj.players) {
                        if (obj.players[index].match(new RegExp('^' + this.search, 'ig'))) {
                            return true;
                        }
                    }

                    return false;
                });
            }
        },
        methods: {
            isSelected(id) {
                return id === this.selected;
            },
            selectTopic(id) {
                this.$events.fire('topicSelected',id);
                this.selected = id;
            },
        }
    }
</script>

<style>
    .story-search {
        z-index: 10;
        box-shadow: 0 8px 6px -6px black;
    }
    .story-list {
        margin-top: 8rem;
        z-index: 1
    }
</style>