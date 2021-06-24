<template>
    <div class="user-profile">
        <div class="user-profile__sidebar">
            <div class="user-profile__user-panel">
                <h1 class="user-profile__username">@{{ user.userName }}</h1> <!--References the user.userName variable from data. Double curly braces are for dynamic data points. Anything from data().return is referencable here using double curly braces-->
                <div class="user-profile__admin-badge" v-if="user.isAdmin"> <!-- v-if stands for if else -->
                    Admin
                </div>
                <div class="user-profile__follower-count">
                    <strong>Followers: </strong> {{ followers }}
                </div>
            </div>
            <CreateTwootPanel @add_twoot="addTwoot"/>
        </div>
        <div class="user-profile__twoots-wrapper">
            <TwootItem
                    v-for="twoot in user.twoots"
                    :key="twoot.id"
                    :username="user.userName"
                    :twoot="twoot" @favourite="toggleFavourite"/> <!-- without :key = error  Elements in iteration expect to have 'v-bind:key' directives -->
        </div>
    </div>
</template>

<script>
    import TwootItem from "./TwootItem";
    import CreateTwootPanel from "./CreateTwootPanel";
    export default {
        name: 'UserProfile',
        components: {CreateTwootPanel, TwootItem},
        data() {
            return {
                followers: 0,
                user: {
                    id: 1,
                    userName: "__mis2",
                    firstName: "Sourav",
                    lastName: "Sett",
                    email: 'sourav241196@gmail.com',
                    isAdmin: true,
                    twoots: [
                        {id: 1, content: "My first twoot! WooHoo"},
                        {id: 2, content:"Twotter is sugoiiii!!! (❁´▽`❁)*✲ﾟ*"}
                    ]
                }
            }
        },
        methods: {
            addTwoot(twoot) {
                this.user.twoots.unshift( {id: this.user.twoots.length + 1, content:twoot});
            }
        }
    };
</script>

<style lang="scss" scoped>
    .user-profile {
        display: grid;
        grid-template-columns: 1fr 3fr;
        /*width: 100%;*/  /*commented this line to prevent horizontal scrolling*/
        padding: 50px 5%;
        overflow: hidden;

        .user-profile__user-panel {
            display: flex; /*changes the layout-structure to flex style*/
            flex-direction: column; /*sets the flex layout-structure to be of column types*/
            margin-right: 50px;
            padding: 10px; /*sets the padding from borders of div .user-profile_iser-panel*/
            background-color: #FFFFFF;
            border-radius: 5px; /*rounded edge*/
            border: 1px solid #dfe3e8; /*sets the width,style,and color of border*/

            .user-profile__admin-badge {
                background-color: rebeccapurple;
                color: white;
                border-radius: 5px;
                margin-right: auto;
                padding: 0 10px;
                font-weight: bold;
            }

            h1 {
                margin: 0; /*removes margin around h1*/
            }
        }
        .user-profile__twoots-wrapper {
            display: grid;
            grid-gap: 10px;
            margin-bottom: auto;
            padding-left: 10px;
        }
    }
</style>
