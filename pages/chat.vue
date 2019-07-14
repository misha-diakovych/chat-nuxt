<template>
    <div class="c-wrap">
        <div class="c-chat" ref="block">
            <Message v-for="m in messages" :key="m.id" :name="m.name" :text="m.text" :owner="m.id === user.id"/>
        </div>
        <div class="c-form">
            <ChatForm/>
        </div>
    </div>
</template>

<script>
    import { mapState } from 'vuex'
    import Message from '../components/Message'
    import ChatForm from '../components/ChatForm'

    export default {
        middleware: ["chat"],
        head () {
            return {
                title: `Room ${this.user.room}`
            }
        },
        computed: mapState(["user", "messages"]),
        components: {
            Message,
            ChatForm
        },
        watch: {
            messages () {
                setTimeout( () => {
                    this.$refs.block.scrollTop = this.$refs.block.scrollHeight
                }, 0)
            }
        }
    }
</script>

<style scoped>
    .c-wrap {
        width: 100%;
        position: relative;
        overflow: hidden;
        height: 100%;
    }
    .c-chat {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 80px;
        overflow-y: auto;
        padding: 1rem;
    }
    .c-form {
        position: absolute;
        bottom: 0;
        left: 0;
        right: 0;
        padding: 1rem;
        height: 80px;
        background: #212121;
    }

</style>