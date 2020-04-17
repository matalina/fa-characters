<template>
    <div class="w-100">
        <div class="story-search container bg-white position-fixed">
          <div class="row">
            <div class="col">
              <button class="btn btn-outline-dark float-right mt-1 mr-1" @click="close()">
                <i class="far fa-fw fa-times-circle"></i>
              </button>
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
        </div>
        <div class="story-bio" v-show="bio !== null">
            <div class="">
                <ul class="list-group">
                    <li :class="{ active: isSelected(bio) }"
                        class="list-group-item my-2"
                        @click="selectTopic(bio)"
                    >
                        {{ name }}'s Biography
                    </li>
                </ul>
            </div>
        </div>
        <div class="story-list">
                <ul v-for="topic in filteredStory"
                    class="list-group"
                >
                    <li :class="{ active: isSelected(topic.tid) }"
                        class="list-group-item my-2"
                        @click="selectTopic(topic.tid, topic.subject)"
                    >
                        {{ topic.subject}}<br/>
                        <small>
                            on
                            <template v-for="forum in topic.forum">
                                {{ forum }}
                            </template><br/>
                        </small>
                        Players:<br/>
                        <template v-for="(avatar, player) in topic.players">
                            <span class="float-left image-cropper mr-2">
                            <img :src="getAvatar(avatar)"
                                 class=""
                                 :alt="player"
                            /></span>
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
        props: ['story', 'bio', 'name'],
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
                    if(obj.subject.match(new RegExp(this.search, 'ig'))) {
                        return true;
                    }
                    let keys = Object.keys(obj.players);
                    for(let index in keys) {

                        if (keys[index].match(new RegExp(this.search, 'ig'))) {
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
            selectTopic(id,title) {
                this.$events.fire('topicSelected',{id,title});
                this.selected = id;
            },
          close() {
              this.$events.fire('close','story');
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
    .story-search {
        z-index: 10;
        box-shadow: 0 8px 6px -6px black;
    }
    .story-bio {
        margin-top: 8rem;
        z-index:1;
    }
    .story-list {
        z-index: 1
    }
</style>
