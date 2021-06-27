<template>
    <div class="user-profile">
        <div class="user-profile__sidebar">
            <div class="user-profile__user-panel">
                <h1 class="user-profile__username">@{{ state.user.userName }}</h1> <!--References the state.user.userName variable from setup(). Double curly braces are for dynamic data points. Anything from setup().return is referencable here using double curly braces-->
                <div class="user-profile__admin-badge" v-if="state.user.isAdmin"> <!-- v-if stands for if else -->
                    Admin
                </div>
                <div class="user-profile__follower-count">
                    <strong>Followers: </strong> {{ followers }}
                </div>
            </div>
            <CreateTwootPanel @add_twoot="addTwoot"/>
            <button @click="test">
                Fetch!
            </button>
        </div>
        <div class="user-profile__twoots-wrapper"  ref='scrollComponent'>
            <TwootItem
                    v-for="twoot in state.user.twoots"
                    :key="twoot.id"
                    :username="state.user.userName"
                    :twoot="twoot" @favourite="toggleFavourite"/> <!-- without :key = error  Elements in iteration expect to have 'v-bind:key' directives -->
        </div>
    </div>
</template>

<script>
    import TwootItem from "./TwootItem";
    import CreateTwootPanel from "./CreateTwootPanel";
    import {reactive,ref,onMounted,onUnmounted,onBeforeMount} from 'vue';
    export default {
        name: 'UserProfile',
        components: {CreateTwootPanel, TwootItem},
        setup () {
            const state = reactive({
                followers: 0,
                user: {
                    id: 1,
                    userName: "__mis2",
                    firstName: "Sourav",
                    lastName: "Sett",
                    email: 'sourav241196@gmail.com',
                    isAdmin: true,
                    twoots: []
                },
                cursor: 0
            })

            function addTwoot(twoot) {
                state.user.twoots.unshift( {id: state.user.twoots.length + 1, content:twoot});
            }


            async function test() {
                const res = await fetch('https://dukaan.bubbleapps.io/version-test/api/1.1/obj/product?limit=20&cursor='+state.cursor);
                const data = await res.json();
                const myArr = data.response.results;
                myArr.forEach(v => addTwoot(`
                ${v.brand} ${v.name}
                ${v.description}
                Weight: ${v.weight}
                Rs. ${v['selling price']}`));
                state.cursor += 20
            }

            onBeforeMount(()=> {
                test();
            })

            onMounted(() => {
                window.addEventListener("scroll", handleScroll)
            })

            onUnmounted(() => {
                window.removeEventListener("scroll", handleScroll)
            })
            const handleScroll = () => {
                let element = scrollComponent.value;
                if ( element.getBoundingClientRect().bottom < window.innerHeight ) {
                    test()
                }
            }

            const scrollComponent = ref(null)
            return {
                scrollComponent,
                state,
                addTwoot,
                test
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
