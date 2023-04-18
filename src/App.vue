<template>
    <div class="admin">
        <p>-- Admin Area --</p>
        <p>
            <span>theme: </span
            ><select
                name="theme-select"
                id="theme-select"
                v-model="theme"
                @select="updateLocal"
            >
                <option :value="true">Dark</option>
                <option :value="false">Light</option>
            </select>
        </p>
        <p>
            <span>user: </span>
            <select name="user-select" id="user-select" v-model="user">
                <option value="John Doe">John</option>
                <option value="Jane Doe">Jane</option>
            </select>
        </p>
    </div>
    <div class="messenger-wrap" :class="{ dark: theme }">
        {{ theme }}
        <div class="messenger">
            <div class="messages" v-for="item in listItems" :key="item.id">
                <VueMessage
                    v-if="item.user === 'John Doe'"
                    :message="item.message"
                    :user="item.user"
                />
                <VueMessage
                    v-else
                    :message="item.message"
                    :received="true"
                    :user="item.user"
                />
            </div>
            <div class="new">
                <input
                    class="new-message"
                    type="text"
                    name="new-message"
                    v-model="message"
                    @keyup.enter="sendMessage"
                />
                <button class="send" @click="sendMessage()">send</button>
            </div>
        </div>
    </div>
</template>

<script>
import VueMessage from "./components/VueMessage.vue";

export default {
    name: "App",
    data() {
        return {
            listItems: [],
            message: "",
            user: "John Doe",
            theme: false,
        };
    },
    mounted() {
        fetch("http://localhost:3000/messages")
            .then((res) => res.json())
            .then((data) => (this.listItems = data))
            .catch((err) => console.log(err.message));
        if (localStorage.theme) {
            this.theme = JSON.parse(localStorage.theme);
        }
    },
    components: {
        VueMessage,
    },
    methods: {
        sendMessage() {
            fetch("http://localhost:3000/messages", {
                method: "POST",
                body: JSON.stringify({
                    message: this.message,
                    user: this.user,
                }),
                headers: {
                    "Content-type": "application/json; charset=UTF-8",
                },
            })
                .then((response) => response.json())
                .then((data) => {
                    console.log(data);
                    this.listItems.push(data);
                    this.message = "";
                });
        },
    },
    watch: {
        theme(val) {
            localStorage.theme = val;
        },
    },
};
</script>

<style lang="less">
html,
body {
    margin: 0;
}
.admin {
    background-color: #ddd;
    padding: 5px 20px;
    margin: 0;
}
.messenger-wrap {
    margin: 10px;
}
.messenger {
    max-width: 600px;
    margin: 0 auto;
}
.new {
    padding: 20px 0;
    display: grid;
    grid-template-columns: 3fr 80px;
    grid-template-rows: 2fr;
    grid-column-gap: 10px;
    grid-row-gap: 20px;
    justify-items: stretch;
    align-items: stretch;
    clear: both;
}
.new-message {
    font-size: medium;
    box-sizing: border-box;
    padding: 4px;
    width: 100%;
    border: 2px solid #ddd;
    border-radius: 4px;
    display: block;
}
.send {
    border-radius: 20px;
    border: none;
    width: 100%;
    background-color: rgb(47, 144, 255);
    color: #fff;
}

// dark theme
.messenger-wrap.dark {
    background-color: #333;
}
.dark .new-message {
    background-color: #666;
    color: #fff;
    border: none;
}
</style>
