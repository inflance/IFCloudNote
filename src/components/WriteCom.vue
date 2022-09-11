<script>
    import { marked } from 'marked';

    export default {
        data() {
            return {
                count: 0,
                indexOfLine: 0,
                nums:[{id:"w0", nativeText:"## 123", translateText:""},],
                id:''
            }
        },
        mounted(){
            this.Translate()
        }
        ,methods:{
            //添加一行
            AddLine: function(){
                
                this.nums.push({id:'w' + (this.indexOfLine += 2),nativeText:"## 1234", translateText:""});
                this.Translate();
                console.log(this.nums);
            },
            //将markdown文本翻译
            Translate: function(){
                this.nums.forEach(
                    function(num){
                        num.translateText = marked(num.nativeText);
                    }
                )
            },
            //阻止浏览器换行
            HandlePushKeyword: function(event){
                if (event.keyCode === 13) {
                    event.preventDefault(); // 阻止浏览器默认换行操作
                    return false;
                }
            },
            Alert : function(id){
                this.id = id;
                alert(id)
            },
            getEditableDivFocus: function (id) {
                this.id = id;
                alert(id)
            }

        },
        render: function (createElement) {
            return createElement('h1', this.blogTitle)
        }
    }
</script>

<template>
    <div id = "write" contenteditable="true" @keyup.enter="AddLine" @keydown="HandlePushKeyword($event)">
        <p v-for="num in nums"  v-bind:id="num.id" contenteditable="true" spellcheck="true"><span contenteditable="true" tabindex="0"  @focus="getEditableDivFocus(num.id)" @blur="Alert(num.id)">{{num.nativeText}}</span></p>
        
    </div>
    <button @click="AddLine">add</button>
</template>

<style>
    #write{
        background-color: wheat;
        color:rgb(0, 1, 1);

    }
</style>



