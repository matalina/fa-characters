<template>
    <div class="col-lg-3">
        <ul v-for="topic in story"
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
</template>

<script>
    export default {
        name: 'story',
        props: ['story'],
        data () {
            return {
                selected: null,
            };
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

</style>