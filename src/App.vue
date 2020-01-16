<template>
    <div id="app">
        <div class="group">
            <div class="title">В работе</div>
            <draggable class="group_drag" :list="list1" group="people">
                <div @click="modalshow(element.name,list1)"
                        class="group_item"
                        v-for="(element) in list1"
                        :key="element.id"
                        :data-id="element.id"
                >
                    {{ element.name | truncate(71, '...') }}
                </div>
            </draggable>
            <template v-if="txt1">
                <textarea class="add_item_name_new" v-model="name1"></textarea>
                <div class="list_btn">
                    <div class="btn_add">
                        <button @click="add_item(name1,list1)">Добавить карточку</button>
                    </div>
                    <div class="btn_reset">
                        <button @click="txt1=false">Отмена</button>
                    </div>
                </div>
            </template>
            <div class="add_item" @click="txt1=true">+ Добавить еще одну карточку</div>
        </div>

        <div class="group">
            <div class="title">На проверке</div>
            <draggable class="group_drag" :list="list2" group="people">

                <div @click="modalshow(element.name,list2)"
                        class="group_item"
                        v-for="(element) in list2"
                        :key="element.id"
                        :data-id="element.id"
                >
                    {{ element.name | truncate(71, '...') }}
                </div>

            </draggable>
            <template v-if="txt2">
                <textarea class="add_item_name_new" v-model="name2">></textarea>
                <div class="list_btn">
                    <div class="btn_add">
                        <button @click="add_item(name2,list2)">Добавить карточку</button>
                    </div>
                    <div class="btn_reset">
                        <button @click="txt2=false">Отмена</button>
                    </div>
                </div>
            </template>
            <div class="add_item" @click="txt2=true">+ Добавить еще одну карточку</div>
        </div>

        <div class="group">
            <div class="title">Выполнено</div>
            <draggable class="group_drag" :list="list3" group="people">

                <div @click="modalshow(element.name,list3)"
                        class="group_item"
                        v-for="(element) in list3"
                        :key="element.id"
                        :data-id="element.id"
                >
                    {{ element.name | truncate(71, '...') }}
                </div>

            </draggable>
            <template v-if="txt3">
                <textarea class="add_item_name_new" v-model="name3">></textarea>
                <div class="list_btn">
                    <div class="btn_add">
                        <button @click="add_item(name3,list3)">Добавить карточку</button>
                    </div>
                    <div class="btn_reset">
                        <button @click="txt3=false">Отмена</button>
                    </div>
                </div>
            </template>
            <div class="add_item" @click="txt3=true">+ Добавить еще одну карточку</div>
        </div>

        <modal v-if="modaltru" @close="close">
            <template v-slot:header>
               {{headertitle}}
            </template>
        </modal>
    </div>
</template>

<script>
    import draggable from 'vuedraggable';
    import modal from './components/modal';

    export default {
        name: 'app',
        components: {draggable, modal},
        data() {
            return {
                headertitle:'',
                modaltru: false,
                txt1: false,
                txt2: false,
                txt3: false,
                activelist:null,
                name1: '', name2: '', name3: '',
                list1: [
                    {name: "Текст1", id: 1},
                    {
                        name: " длинная страница ушла в правый блок сломав всю верстку и блок работать стал очень плохо",
                        id: 2
                    },
                ],
                list2: [
                    {name: "ТЕкстовка 2", id: 3},
                ],
                list3: [
                    {name: "ТЕкст3", id: 4},
                ]
            };
        },
        filters: {
            truncate: function (text, length, suffix) {
                text.length > length ? suffix : suffix = ''
                return text.substring(0, length) + suffix;
            },
        },
        methods: {
            modalshow(names,list){
                this.$router.push('/modal')
                this.headertitle = names
                this.activelist = list
                this.modaltru=true

            },
            close(txt) {

                this.$router.push('/')
                this.modaltru = false
                if (txt!=null && txt!='') {
                    this.activelist.forEach(item=>{
                        if (item.name==this.headertitle){
                            item.name = txt
                        }
                    })
                }

                this.activelist=null
            },
            generatid() {
                let ids = new Date().getTime()
                return ids
            },
            add_item(names, item) {
                if (names != '') {
                    item.push({name: names, id: this.generatid()})
                    this.name1 = '', this.name2 = '', this.name3 = ''
                } else {
                    alert('Заполните название')
                }

            }
        }
    }
</script>

<style lang="scss">
    body {
        background: #1d8b77;
    }

    #app {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .group {
        background: #d4d4d4;
        width: 300px;
        border-radius: 6px;
        box-shadow: 0 0 1px #d4d4d4;
        padding: 10px;
        margin: 0 15px;

        .group_item {
            background: white;
            padding: 6px 12px;
            margin: 5px 0;
            box-shadow: 0 0 2px #ccc;
            border-radius: 2px;
            cursor: pointer;

            &:hover, &:active, &:focus {
                cursor: pointer;
            }

            &.sortable-chosen {
                cursor: grab !important;
                background: aquamarine;
            }

            &.sortable-ghost {
                background: darkgrey;
                text-indent: -999999px;
            }
        }

        .list_btn {
            display: flex;
            align-items: center;

            .btn_add {
                flex: 2;

                button {
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
            }

            .btn_reset {
                flex: 1;

                button {
                    width: 100%;
                    background: none;
                    border: 0;
                    outline: none;

                }
            }
        }

        .add_item_name_new {
            width: 96%;
            padding: 2%;
            margin: 0;
            height: 45px;
            resize: none;
        }

        .add_item {
            color: #4b4747;
            text-align: center;
            padding: 5px 0 5px 0;
            margin: 10px 0 0 0;
            display: block;
            cursor: pointer;

            &:hover, &:focus, &:active {
                background: #ccc;
            }
        }
    }
</style>
