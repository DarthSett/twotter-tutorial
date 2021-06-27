<template>
    <form class="create-twoot-panel" @submit.prevent="createNewTwoot" :class="{'--exceeded':newTwootCharacterCount > 180}">
        <label for="newTwoot" ><strong>New Twoot</strong> <small>({{newTwootCharacterCount}}/180) </small> </label> <!--small gets turned to red when exceeded class is activated (check style)-->
        <textarea id="newTwoot" rows="4" v-model = "state.newTwootContent"/>
        <div class="create-twoot-panel__submit">
            <div class="create-twoot-type">
                <label for="newTwootType"><strong>Type: </strong></label>
                <select id="newTwootType" v-model = "state.selectedTwootType"> <!--drop down-->
                    <option :value="option.value" v-for="(option,index) in state.twootTypes" :key="index">
                        {{ option.name }}
                    </option>
                </select>
            </div>

            <button :disabled='newTwootCharacterCount > 180'> <!--disable button if wordcount above 180-->
                Twoot!
            </button>
        </div>
    </form>
</template>

<script>
    import {reactive, computed} from 'vue';

    export default {
        name: 'CreateTwootPanel',
        setup(props, ctx) {
            const state = reactive({
                newTwootContent: '',
                selectedTwootType:'instant',
                twootTypes: [
                    {value: 'draft', name: 'Draft'},
                    {value: 'instant', name: 'Instant Twoot'}
                ],
            })

            const newTwootCharacterCount = computed(() => state.newTwootContent.length)

            function createNewTwoot() {
                if (state.newTwootContent && state.selectedTwootType !== 'draft') {
                    ctx.emit('add_twoot', state.newTwootContent);      //$emit emits the add_twoot event along with the data newTwootContent
                    state.newTwootContent = '';
                }
            }

            return {
                state,
                newTwootCharacterCount,
                createNewTwoot,
            }
        },
        // watch: {
        //     followers(newFollowerCount, oldFollowerCount) {
        //         if (oldFollowerCount > newFollowerCount) {
        //             console.log("new follower")
        //         }
        //     }
        // }
    }
</script>

<style lang="scss" scoped>
    .create-twoot-panel {
        margin-top: 20px;
        padding: 20px 0;
        display: flex;
        flex-direction: column;

        textarea {
            border-radius: 5px; /*rounded edge*/
            border: 1px solid #dfe3e8; /*sets the width,style,and color of border*/
        }

        .create-twoot-panel__submit{
            display: flex;
            justify-content: space-between;

            .create-twoot-type {
                padding: 10px 0;
            }

            button {
                padding: 5px 20px;
                margin: auto 0;
                border-radius: 5px;
                border: none;
                background-color: deeppink;
                color: white;
                font-weight: bold;
            }
        }
        &.--exceeded{
            color: red;
            border-color: red;
            .create-twoot-panel__submit{
                button {
                    background-color: red;
                    color: #FFFFFF;
                }
            }
        }
    }


</style>
