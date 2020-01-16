<template>
    <transition name="modal">
        <div class="modal-mask" @click="$emit('close')">
            <div class="modal-wrapper">
                <div class="modal-container" @click.stop>

                    <div class="modal-header">
                        <slot name="header"></slot>
                    </div>

                    <div class="modal-body">
                        <slot name="body">
                            <textarea v-model="text"></textarea>
                        </slot>
                    </div>

                    <div class="modal-footer">
                       <button @click="saveitem()">Сохранить</button>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>
    export default {
        name: "modal",
        data() {
            return{
                text:'',
            }
        },
        methods:{
            saveitem(){
                this.$emit('close',this.text)
            }
        }
    }
</script>

<style scoped lang="scss">
    .modal-mask {
        position: fixed;
        z-index: 9998;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, .5);
        display: table;
        transition: opacity .3s ease;
    }

    .modal-wrapper {
        display: table-cell;
        vertical-align: middle;
    }

    .modal-container {
        width: 300px;
        margin: 0px auto;
        padding: 20px 30px;
        background-color: #fff;
        border-radius: 2px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
        transition: all .3s ease;
        font-family: Helvetica, Arial, sans-serif;
    }

    .modal-header h3 {
        margin-top: 0;
        color: #42b983;
    }

    .modal-body {
        margin: 20px 0;
    }

    .modal-default-button {
        float: right;
    }


    .modal-enter {
        opacity: 0;
    }

    .modal-leave-active {
        opacity: 0;
    }

    .modal-enter .modal-container,
    .modal-leave-active .modal-container {
        -webkit-transform: scale(1.1);
        transform: scale(1.1);
    }
    textarea{
        width: 98%;
        padding: 1%;
        border: 1px solid #ccc;
        resize: none;
        height: 55px;
    }
    .modal-footer button {
        background: blue;
        display: block;
        color: white;
        border: 0;
        padding: 7px 20px;
        border-radius: 3px;
        box-shadow: 0 0 4px #ccc;
        width: 90%;
        outline: none;
    }
</style>