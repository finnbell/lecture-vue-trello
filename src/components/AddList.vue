<template>
  <div class="add-list">
    <input class="form-control" v-if="isAddList" type="text" v-model="inputTitle" 
        ref="inputTitle" @blur="restore" @keyup.enter="onSubmitTitle">
    <a v-else @click="onAddList">&plus; Add another list</a>
  </div>
</template>

<script>
import {mapState, mapActions } from 'vuex'

export default {
    data() {
        return {
            isAddList: false,
            inputTitle: '',
        }
    },
    computed: {
        ...mapState({
            board: 'board',
        })
    },

    methods: {
        ...mapActions([
            'ADD_LIST'
        ]),
        onAddList() {
            this.isAddList = true
            this.$nextTick( () => this.$refs.inputTitle.focus() )
        },
        onSubmitTitle() {
            
            this.inputTitle = this.inputTitle.trim()
            if(!this.inputTitle) return this.restore()
            
            const title = this.inputTitle
            const boardId = this.board.id
            const lastList = this.board.lists[this.board.lists.length -1]
            const pos = lastList ? lastList.pos * 2 : 65535

            this.ADD_LIST( { title, boardId ,pos}) 
                .then( _ => this.restore() )
            
        },
        restore() {
            this.isAddList = false
            this.inputTitle = ''
        }
    }
    
}
</script>

<style>
.add-list {
    background-color: rgba(0,0,0,.1);
    padding:12px;
    color: #ddd;
    transition: all, 3s
}
.add-list:hover,
.add-list:focus {
    background-color: rgba(0,0,0.3);
}
</style>