<template>
    <Card style="flex: 1 0 auto;margin-left: 16px;">
        <p slot="title">
            <span>{{titleText}}</span>
            <span style="color: #ccc;" v-if="chatRoomStatus == 0">{{statusText}}</span>
            <span style="color: #19be6b;" v-else>{{statusText}}</span>
        </p>
        <p slot="extra">
            <span>观看人数：{{number}} </span>
            <span style="margin-left: 8px;color: #19be6b">开始时间：{{startDate}}</span>
        </p>

        <div class="barrage-container">
            <Barrage class="barrage-box" ref="barrage"></Barrage>

            <div class="barrage-input-box" v-if="showInput">
                <Poptip trigger="hover" title="发送者名称">
                    <div slot="content">
                        <p>请勿滥用</p>
                        <p>请勿Diss小偶像</p>
                        <p>请勿KY</p>
                    </div>
                    <Input style="width:160px;" v-model="senderName"
                           placeholder="发送者名称"/>
                </Poptip>

                <Input v-model="content" placeholder="请填写弹幕内容"
                       style="margin-left: 8px;" clearable
                       @on-enter="sendBarrage"/>

                <Button type="primary" style="margin-left: 8px;"
                        @click="sendBarrage"
                        :disabled="sendDisabled">
                    {{sendText}}
                </Button>
            </div>
        </div>
    </Card>
</template>

<script>
    import Barrage from './Barrage';
    import Tools from "../assets/js/tools";

    export default {
        name: "BarrageBox",
        props: {
            number: {
                type: Number,
                required: true,
                default: 0
            },
            sendDisabled: {
                type: Boolean,
                required: true,
                default: false
            },
            sendBarrage: {
                type: Function,
                required: true
            },
            sendText: {
                type: String,
                required: true,
                default: ''
            },
            showInput: {
                type: Boolean,
                required: true,
                default: false
            },
            chatRoomStatus: {
                type: Number,
                required: true,
                default: ''
            },
            startTime: {
                type: Number,
                required: true
            },
            isLive: {
                type: Boolean,
                required: true
            }
        },
        components: {Barrage},
        data() {
            return {
                senderName: Tools.getSenderName() == undefined || Tools.getSenderName() == null ? '' : Tools.getSenderName(),
                content: ''
            };
        },
        computed: {
            startDate() {
                return new Date(this.startTime).format('yyyy-MM-dd hh:mm');
            },
            titleText() {
                return this.isLive ? '聊天室' : '弹幕';
            },
            statusText() {
                if (this.isLive) {
                    if (this.chatRoomStatus == 0){
                        return '未连接'
                    }
                    return '已连接';
                } else {
                    if (this.chatRoomStatus == 0){
                        return '未加载'
                    }
                    return '已加载';
                }
            }
        },
        methods: {
            shoot: function (barrage) {
                this.$refs.barrage.shoot(barrage);
            },
            clear: function () {
                this.$refs.barrage.clear();
            }
        }
    }
</script>

<style scoped>

</style>
