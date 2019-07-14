<template>
    <div v-if="name === 'admin'" class="system">
        <p class="text-xs-center">{{text}}</p>
        <hr>
    </div>
    <div v-else class="wrap" >
        <div class="mes" :class="{ owner }">
            <small><strong>{{name}}</strong></small>
            <p>{{text}}</p>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Message",
        props: {
            name: String,
            text: String,
            owner: {
                type: Boolean,
                default: false
            }
        }
    }
</script>

<style lang="scss" scoped>
    .system {
        margin-bottom: 1rem;
        p {
            margin-bottom: 1rem;
        }
    }
    .wrap {
        display: flex;
        flex-direction: column;
    }
    .mes {
        padding: 1rem;
        width: 60%;
        margin: 0 1rem 1rem 1rem;
        box-shadow: 0 1px 0 0 rgba(50, 50, 50, 0.3);
        border-radius: 4px;
        background: #1976d2;
        position: relative;
        p {
            margin-bottom: 0;
        }
    }
    .owner {
        background: white;
        color: black;
        align-self: flex-end;
    }
</style>