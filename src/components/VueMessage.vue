<template>
    <div class="vue-message" :class="{ received: received }">
        {{ message }}
    </div>
    <div class="vue-user" :class="{ received: received }">{{ user }}</div>
</template>

<script>
export default {
    name: "VueMessage",
    props: {
        received: Boolean,
        message: String,
        user: String,
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.vue-user,
.vue-message {
    font-family: Arial, Helvetica, sans-serif;
    float: right;
    clear: both;
}
.vue-user {
    margin: 0 10px 0 0;
    font-size: smaller;
}
.vue-message {
    margin: 10px 0 0;
    padding: 10px;
    border-radius: 20px;
    background-color: #eee;
    width: 60%;
    max-width: 400px;
}
.vue-user.received,
.vue-message.received {
    float: left;
}
.vue-user.received {
    margin: 0 0 0 10px;
}
.vue-message.received {
    background-color: rgb(47, 144, 255);
    color: #fff;
}
.dark .vue-message:not(.vue-message.received) {
    background-color: #666;
    color: #fff;
}
.dark .vue-user {
    color: #ddd;
}
</style>
