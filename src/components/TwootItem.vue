<template>
    <div class='scrolling-component' ref='scrollComponent'>
        <div class="twoot-item">
            <div class="user-profile__twoot">
                <div class="twoot-item__user">
                    @{{ username }}
                </div>
                <div class="twoot-item__content">
                    {{ twoot.content }}
                </div>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        name: "TwootItem",
        props: {
            username: {
                type: String,
                required: true
            },
            twoot: {
                type: Object,
                required: true
            }
        }
    }
</script>

<style lang="scss" scoped>
    .twoot-item {
        /*margin-right: 80px;*/
        /*margin-bottom: 5px;*/
        /*width: 100%;*/
        padding: 20px;              /*sets the padding from borders of div .user-profile_user-panel*/
        background-color: #FFFFFF;
        border-radius: 5px;         /*rounded edge*/
        border: 1px solid #dfe3e8;  /*sets the width,style,and color of border*/
        box-sizing: border-box;
        cursor: pointer;
        transition: all 0.25s ease;

        .twoot-item__user {
            font-weight: bold;
            font-size: larger;
        }
        .twoot-item__content {
            font-style: italic;
        }

        &:hover{
            transform: scale(1.1,1.1); /*hover mechanism*/
        }
    }


</style>
