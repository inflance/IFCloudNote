<script>
import { marked } from 'marked';

export default {
    data() {
        return {
            count: 0,
            indexOfLine: 0,
            nums: [
                { id: "w0", nativeText: "## 123", translateText: "" },
            ],
            id: '',
            range: {
                rangeId:'w0',
                rangeStart: '', 
                rangeEnd: ''
            },
        }
    },
    mounted() {
        this.Translate()
    },
    updated() {

        this.$nextTick(() => {
            document.onselectionchange = () => {
                this.range = {
                    rangeId:'w0',
                    rangeStart: '', 
                    rangeEnd: ''
                }
                const selection = document.getSelection();
                for (let i = 0; i < selection.rangeCount; i++) {
                    var rangeStart = selection.getRangeAt(i).startOffset
                    var rangeEnd = selection.getRangeAt(i).endOffset
                    var rangeId = selection.getRangeAt(i).commonAncestorContainer.parentElement.parentElement.getAttribute('id')
                    this.range = {rangeId:rangeId, rangeStart:rangeStart, rangeEnd:rangeEnd}
                    console.log(this.range)
                }
            }
            this.nums[this.range.rangeId].nativeText = document.querySelector('#'+this.range.rangeId).firstChild.innerHTML
            
        })
    },
    methods: {
        //添加一行
        AddLine: function () {
            let i = 0;
            this.nums.forEach((num, index) => {//获取光标所在的数组位置
                if(num.id === this.range.rangeId){
                    i = index;
                }
            });
            //在光标位置后插入一行
            this.nums.splice(i+1, 0, { id: 'w' + (this.indexOfLine += 2), nativeText: ("## "+ i), translateText: "" })
            this.Translate();
        },
        //将markdown文本翻译成HTML
        Translate: function () {
            this.nums.forEach((num) => {
                    num.translateText = marked(num.nativeText);
                }
            )
        },
        //阻止浏览器换行
        HandlePushKeyword: function (event) {
            if (event.keyCode === 13) {
                event.preventDefault(); // 阻止浏览器默认换行操作
                return false;
            }
        },
        Alert: function (id) {
            this.id = id;
            alert(id)
        },
        getEditableDivFocus: function (id) {
            this.id = id;
            alert(id)
        }, changeE: function () {
            alert(123)
        }
    }
}
</script>

<template>
    <div id="write" contenteditable="true" @keyup.enter="AddLine" @keydown="HandlePushKeyword($event)">
        <p v-for="num in nums" v-bind:id="num.id" contenteditable="true" @change="changeE">
            <span>{{num.nativeText}}</span>
            <span v-html="num.translateText"></span>
        </p>
    </div>
    <button @click="AddLine">add</button>
</template>

<style>
#write {
    background-color: wheat;
    color: rgb(0, 1, 1);
    width: 80vw;
    height: 90vh;

}
</style>



