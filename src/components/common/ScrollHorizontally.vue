<template>
    <div class="scroll-wapper" ref="scrollWapper">
        <ul class="scroll-content" ref="scrollContent" style="padding-bottom: 4px">
            <li class="scroll-item scroll-item-shadow" :style="item_style" v-for="(item, i) in data" :key="i">
                <router-link :to="'/goods/' + item.goods_id" class="d-block">
                    <div class="topic1-goods-images" style="height: 80px; width: 80px">
                        <van-image fit="cover" width="80" height="80" :src="item.goods.master_image" />
                    </div>
                    <div style="padding: 4px 0">
                        <div class="topic1-goods-title van-ellipsis fs-10 text-21 text-left" style="line-height: 14px">
                            {{item.goods.name}}
                        </div>
                        <del class="topic1-goods-del-price d-block fs-10 text-left van-ellipsis" style="color: #9E9E9E">
                            原价:￥{{item.original_price}}
                        </del>
                        <div class="topic1-goods-price text-left fs-12" style="line-height: 12px">
                            <strong class="text-danger van-ellipsis">￥{{item.activity_price}}</strong>
                        </div>
                    </div>
                </router-link>
            </li><li class="item-more fs-12" v-if="more_url">
                <router-link :to="more_url" class="d-block text-9e">
                    <div class="item-more-text">
                        点击查看更多
                    </div>
                    <div>
                        <img src="/img/right-arrow.png" alt="">
                    </div>
                    <div class="text-white">
                        点击查看
                    </div>
                </router-link>
            </li>
        </ul>
    </div>
</template>

<script>
    import BScroll from 'better-scroll'
    export default {
        props: {
            data: {
                type: Array,
                default: []
            },
            more_url: {
                type: String,
                default: ''
            },
            item_style: {
                type: Object,
                default: function() {
                    return {'width': '80px'}
                }
            },
            width: {
                type: Number,
                default: 80
            },
            boxStyle: {
                type: Object,
                default: function() {
                    return {'padding': "0 15px"}
                }
            }
        },
        components: {
            BScroll,
        },
        data() {
            return {
                // scroll: value
            }
        },
        methods: {
            personScroll() {
                // 默认有六个li子元素，每个子元素的宽度为120px
                let width = this.data.length * (this.width + 5);
                if (this.more_url) width += 58
                this.$refs.scrollContent.style.width = width + "px";
                // this.$nextTick 是一个异步函数，为了确保 DOM 已经渲染
                this.$nextTick(() => {
                    if (!this.scroll) {
                        this.scroll = new BScroll(this.$refs.scrollWapper, {
                            startX: 0,
                            click: true,
                            scrollX: true,
                            // 忽略竖直方向的滚动
                            scrollY: false,
                            // eventPassthrough: "vertical"
                        });
                    } else {
                        this.scroll.refresh();
                    }
                });
            }
        },
        created () {
            
        },
        mounted () {
            this.$nextTick(() => {
                this.personScroll();
            });
        },
    }
</script>

<style lang="scss">
.scroll-wapper {
    width: 100%;
    overflow: hidden;
}
.scroll-item {
    display: inline-block; 
    margin-right: 5px;
}
.item-more {
    text-align: center;
    display: inline-block; 
    padding: 0 10px;
    width: 48px;
    // height: 100%;
}
.item-more-text {
    // height: 80px;
}
.scroll-item .van-image img{
    border-top-left-radius: 6px;
    border-top-right-radius: 6px;
}
.scroll-item-shadow {
    box-shadow: 1px 1px 3px #f3f3f3;
}
</style>