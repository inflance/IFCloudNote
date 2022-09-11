<script>
import { marked } from 'marked';

let aaa = 0
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
                rangeId: 'w0',
                rangeStart: 0,
                rangeEnd: 0
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
                    rangeId: '',
                    rangeStart: 0,
                    rangeEnd: 0
                }
                const selection = document.getSelection();

                for (let i = 0; i < selection.rangeCount; i++) {
                    const rangeStart = selection.getRangeAt(i).startOffset
                    const rangeEnd = selection.getRangeAt(i).endOffset
                    const rangeId = selection.getRangeAt(i).commonAncestorContainer.parentElement.parentElement.getAttribute('id');
                    this.range = { rangeId: rangeId, rangeStart: rangeStart, rangeEnd: rangeEnd }
                }

                this.HandleText();
            }
            document.onfocus = ()=>{

            }

        })

    },
    methods: {
        GetNumPos: function (id) {
            let i = 0;
            this.nums.forEach((num, index) => {//获取光标所在的数组位置
                if (num.id === id) {
                    i = index;
                }
            });
            
            return i;
        },
        //添加一行
        AddLine: function () {
            if (this.range.rangeId) {
                let i = this.GetNumPos(this.range.rangeId);
                //在光标位置后插入一行
                this.nums.splice(i + 1, 0, { id: 'w' + (this.indexOfLine += 2), nativeText: ('# 0'), translateText: "" })
                
                this.Translate();
            }

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
        HandleText: function () {
        
            let rid = this.range.rangeId
            if (rid) {
                
                let i = this.GetNumPos(rid)
                this.nums[i].nativeText = document.getElementById(rid).firstChild.textContent;
                this.Translate();
                document
                    .querySelector("#" + rid).lastChild.vHtml = this.nums[i].translateText;
                }

        }
    }
}
</script>

<template>
    <div id="write" contenteditable="true" @keyup.enter="AddLine" @keydown="HandlePushKeyword($event)">
        <p v-for="num in nums" v-bind:id="num.id" contenteditable="true">
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
    display: flex;
    flex-direction: column;
}
</style>



